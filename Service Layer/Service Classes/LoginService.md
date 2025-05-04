import hashlib

class LoginService:
    def __init__(self):
        self.users = {}

    def register(self, username, password):
        if username in self.users:
            return "Username already exists."
        hashed_password = self._hash_password(password)
        self.users[username] = hashed_password
        return "Registration successful."

    def login(self, username, password):
        if username not in self.users:
            return "Invalid username."
        hashed_password = self._hash_password(password)
        if self.users[username] == hashed_password:
            return "Login successful."
        else:
            return "Invalid password."

    def _hash_password(self, password):
         return hashlib.sha256(password.encode()).hexdigest()
