class SubmissionService:
    def __init__(self, database_connection):
        self.db_connection = database_connection

    def submit_data(self, data):
      
        if not self._validate_data(data):
            raise ValueError("Invalid data format")

        try:
            self._save_to_database(data)
            return {"status": "success", "message": "Data submitted successfully"}
        except Exception as e:
            return {"status": "error", "message": f"Failed to submit data: {str(e)}"}

    def _validate_data(self, data):
        
        if not isinstance(data, dict):
            return False
        required_keys = ["user_id", "content", "timestamp"]
        if not all(key in data for key in required_keys):
            return False
        return True

    def _save_to_database(self, data):
        
        cursor = self.db_connection.cursor()
        sql = "INSERT INTO submissions (user_id, content, timestamp) VALUES (%s, %s, %s)"
        values = (data["user_id"], data["content"], data["timestamp"])
        cursor.execute(sql, values)
        self.db_connection.commit()


class AnalyticsService:
    def __init__(self, database_connection):
        self.db_connection = database_connection

    def get_submission_count(self, user_id):
        
        cursor = self.db_connection.cursor()
        sql = "SELECT COUNT(*) FROM submissions WHERE user_id = %s"
        cursor.execute(sql, (user_id,))
        result = cursor.fetchone()
        return result[0] if result else 0


class NotificationService:
    def __init__(self, messaging_service):
        self.messaging_service = messaging_service

    def notify_submission_received(self, user_id):
        
        message = f"Submission received for user {user_id}"
        self.messaging_service.send_message(user_id, message)


class MessagingService:
     def send_message(self, user_id, message):
        
        print(f"Sending message to user {user_id}: {message}")
