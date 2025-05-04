import unittest
from unittest.mock import patch
# Assuminguser registration logic is in a file named 'auth.py'
from auth import register_user, UserExistsError, InvalidEmailError

class TestUserRegistration(unittest.TestCase):
    def test_register_valid_user(self):
        with patch('auth.save_user') as mock_save_user: # Mock the save_user function to avoid actual database interaction
            user_data = {'username': 'testuser', 'email': 'test@example.com', 'password': 'password123'}
            register_user(user_data['username'], user_data['email'], user_data['password'])
            mock_save_user.assert_called_once()

    def test_register_existing_user(self):
         with patch('auth.user_exists', return_value=True): # Mock the user_exists function to simulate an existing user
            user_data = {'username': 'existinguser', 'email': 'test@example.com', 'password': 'password123'}
            with self.assertRaises(UserExistsError):
                register_user(user_data['username'], user_data['email'], user_data['password'])

    def test_register_invalid_email(self):
        user_data = {'username': 'testuser', 'email': 'invalid-email', 'password': 'password123'}
        with self.assertRaises(InvalidEmailError):
            register_user(user_data['username'], user_data['email'], user_data['password'])

    def test_register_short_password(self):
        user_data = {'username': 'testuser', 'email': 'test@example.com', 'password': 'short'}
        with self.assertRaises(ValueError):
             register_user(user_data['username'], user_data['email'], user_data['password'])

if __name__ == '__main__':
    unittest.main()

