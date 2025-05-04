import unittest

class Exam:
    def __init__(self, questions):
        self.questions = questions
        self.answers = {}

    def submit_answer(self, question_number, answer):
      if 1 <= question_number <= len(self.questions):
        self.answers[question_number] = answer
      else:
        raise ValueError("Invalid question number")

    def get_score(self):
        score = 0
        for question_number, answer in self.answers.items():
            if self.questions[question_number - 1].is_correct(answer):
                score += 1
        return score

class Question:
    def __init__(self, text, correct_answer):
        self.text = text
        self.correct_answer = correct_answer

    def is_correct(self, answer):
        return answer == self.correct_answer

class TestExam(unittest.TestCase):
    def setUp(self):
        self.question1 = Question("What is 2 + 2?", "4")
        self.question2 = Question("What is the capital of France?", "Paris")
        self.questions = [self.question1, self.question2]
        self.exam = Exam(self.questions)

    def test_submit_answer(self):
        self.exam.submit_answer(1, "4")
        self.assertEqual(self.exam.answers[1], "4")
        self.exam.submit_answer(2, "Berlin")
        self.assertEqual(self.exam.answers[2], "Berlin")
        with self.assertRaises(ValueError):
          self.exam.submit_answer(3, "London")

    def test_get_score(self):
        self.exam.submit_answer(1, "4")
        self.exam.submit_answer(2, "London")
        self.assertEqual(self.exam.get_score(), 1)
        self.exam.submit_answer(2, "Paris")
        self.assertEqual(self.exam.get_score(), 2)
    
    def test_empty_exam(self):
      empty_exam = Exam([])
      self.assertEqual(empty_exam.get_score(), 0)
      empty_exam.submit_answer(1, "Answer")
      self.assertEqual(empty_exam.get_score(), 0)

    def test_question_is_correct(self):
      self.assertTrue(self.question1.is_correct("4"))
      self.assertFalse(self.question2.is_correct("Berlin"))

if __name__ == '__main__':
    unittest.main()
