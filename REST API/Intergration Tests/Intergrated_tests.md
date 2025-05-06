import requests
import pytest

def test_get_request():
    response = requests.get("https://mathbooster.co.za/api/endpoint")
    assert response.status_code == 200
    assert response.headers["Content-Type"] == "application/json"
    assert "expected_value" in response.json()

def test_post_request():
    data = {"key": "value"}
    response = requests.post("https://mathbooster.co.za/api/endpoint", json=data)
    assert response.status_code == 201
    assert response.json()["key"] == "value"

def test_put_request():
    data = {"key": "new_value"}
    response = requests.put("https://mathbooster.co.za/api/endpoint/1", json=data)
    assert response.status_code == 200
    assert response.json()["key"] == "new_value"

def test_delete_request():
    response = requests.delete("https://mathbooster.co.za/api/endpoint/1")
    assert response.status_code == 204

if __name__ == "__main__":
    pytest.main()