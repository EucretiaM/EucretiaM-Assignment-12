Python
from flask import Flask, request, jsonify

app = Flask(__name__)

homework_assignments = {}

@app.route('/assessment', methods=['POST'])
def create_homework():
    data = request.get_json()
    assignment_id = len(homework_assignments) + 1
    data['id'] = assignment_id
    homework_assignments[assignment_id] = data
    return jsonify(data), 201

@app.route('/assessment/<int:assignment_id>', methods=['GET'])
def get_homework(assignment_id):
    if assignment_id in homework_assignments:
        return jsonify(homework_assignments[assignment_id])
    return jsonify({'message': 'Homework assignment not found'}), 404

@app.route('/assessment/<int:assignment_id>', methods=['PUT'])
def update_homework(assignment_id):
    if assignment_id in homework_assignments:
        data = request.get_json()
        data['id'] = assignment_id
        homework_assignments[assignment_id] = data
        return jsonify(data)
    return jsonify({'message': 'Homework assignment not found'}), 404

@app.route('/assessment/<int:assignment_id>', methods=['DELETE'])
def delete_homework(assignment_id):
    if assignment_id in homework_assignments:
        del homework_assignments[assignment_id]
        return jsonify({'message': 'Homework assignment deleted'})
    return jsonify({'message': 'Homework assignment not found'}), 404

if __name__ == '__main__':
    app.run(debug=True)

