import unittest

def save_exam_data(student_id, answers, filename="exam_results.txt"):
    try:
        with open(filename, "a") as file:  # Append mode
            file.write(f"Student ID: {student_id}\n")
            file.write(f"Answers: {answers}\n")
            file.write("-" * 20 + "\n")
        return True
    except Exception as e:
        print(f"Error saving data: {e}")
        return False

class TestSaveExamData(unittest.TestCase):

    def test_save_data_success(self):
        student_id = "12345"
        answers = ["A", "B", "C", "D"]
        self.assertTrue(save_exam_data(student_id, answers))

        # Verify data is written (optional, file system interaction)
        with open("exam_results.txt", "r") as file:
            content = file.read()
            self.assertIn(student_id, content)
            self.assertIn(str(answers), content)

    def test_save_data_empty_input(self):
      
        self.assertFalse(save_exam_data("", []))
    
    def test_save_data_invalid_filename(self):
        student_id = "12345"
        answers = ["A", "B", "C", "D"]
        self.assertFalse(save_exam_data(student_id, answers, "/invalid/path/exam_results.txt"))

    def test_save_multiple_students(self):
        # Save first student
        student_id_1 = "11111"
        answers_1 = ["A", "A", "A", "A"]
        self.assertTrue(save_exam_data(student_id_1, answers_1))

        # Save second student
        student_id_2 = "22222"
        answers_2 = ["B", "B", "B", "B"]
        self.assertTrue(save_exam_data(student_id_2, answers_2))

        # Verify both are saved
        with open("exam_results.txt", "r") as file:
            content = file.read()
            self.assertIn(student_id_1, content)
            self.assertIn(str(answers_1), content)
            self.assertIn(student_id_2, content)
            self.assertIn(str(answers_2), content)
        
if __name__ == '__main__':
    unittest.main()
