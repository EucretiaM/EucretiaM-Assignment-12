import unittest
from unittest.mock import patch
# Assuming login function is in a module named 'auth'
# and the function itself is named 'login_user'
from auth import login_user 

class TestUserLogin(unittest.TestCase):
    @patch('auth.login_user')
    def test_successful_login(self, mock_login_user):
        # Configure the mock to return True for successful login
        mock_login_user.return_value = True

        # Call the login function with valid credentials
        result = login_user("valid_user", "valid_password")

        # Assert that the login function was called with the correct arguments
        mock_login_user.assert_called_once_with("valid_user", "valid_password")

        # Assert that the result is True, indicating successful login
        self.assertTrue(result)

    @patch('auth.login_user')
    def test_incorrect_password(self, mock_login_user):
        # Configure the mock to return False for incorrect password
        mock_login_user.return_value = False

        # Call the login function with an incorrect password
        result = login_user("valid_user", "wrong_password")

        # Assert that the login function was called with the correct arguments
        mock_login_user.assert_called_once_with("valid_user", "wrong_password")

        # Assert that the result is False, indicating failed login
        self.assertFalse(result)

    @patch('auth.login_user')
    def test_user_not_found(self, mock_login_user):
         # Configure the mock to return False for non-existent user
        mock_login_user.return_value = False
        
        # Call the login function with a non-existent username
        result = login_user("nonexistent_user", "any_password")
        
        # Assert that the login function was called with the correct arguments
        mock_login_user.assert_called_once_with("nonexistent_user", "any_password")
        
        # Assert that the result is False, indicating failed login
        self.assertFalse(result)

if __name__ == '__main__':
    unittest.main()
