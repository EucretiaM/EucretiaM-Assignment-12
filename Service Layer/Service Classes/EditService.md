class EditService:
    def __init__(self, model):
        self.model = model

    def create(self, data):
        """Creates a new record in the data model."""
        new_record = self.model(**data)
        # Assuming the model has a save method
        new_record.save()
        return new_record

    def read(self, record_id):
        """Reads a record from the data model."""
        # Assuming the model has a get method to retrieve by ID
        record = self.model.get(record_id)
        return record

    def update(self, record_id, data):
        """Updates a record in the data model."""
        record = self.model.get(record_id)
        if record:
            for key, value in data.items():
                setattr(record, key, value)
            record.save()
            return record
        return None

    def delete(self, record_id):
        """Deletes a record from the data model."""
        record = self.model.get(record_id)
        if record:
            record.delete()
            return True
        return False
