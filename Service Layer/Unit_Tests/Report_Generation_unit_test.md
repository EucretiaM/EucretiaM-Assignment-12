import unittest
from report_generator import generate_report # Assuming your function is in report_generator.py
import os

class TestReportGeneration(unittest.TestCase):

    def test_generate_report_with_data(self):
        data = {"item1": 10, "item2": 20}
        report = generate_report(data)
        self.assertIn("item1", report)
        self.assertIn("10", report)
        self.assertIn("item2", report)
        self.assertIn("20", report)

    def test_generate_report_empty_data(self):
        data = {}
        report = generate_report(data)
        self.assertIn("No data available", report)

    def test_generate_report_file_output(self):
      data = {"item1": 10, "item2": 20}
      file_path = "test_report.txt"
      generate_report(data, file_path)
      self.assertTrue(os.path.exists(file_path)) #check file exists
      with open(file_path, 'r') as f:
        content = f.read()
        self.assertIn("item1", content)
      os.remove(file_path) #cleanup
