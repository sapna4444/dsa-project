# dsa-project
import json
import os

FILE_NAME = "students.json"


def load_students():
    """Load student records from file."""
    if not os.path.exists(FILE_NAME):
        return []

    try:
        with open(FILE_NAME, "r") as file:
            data = json.load(file)
            return data if isinstance(data, list) else []
    except (json.JSONDecodeError, OSError):
        print("Warning: Could not read student data.")
        return []


students = load_students()


def save_students():
    """Save student records to file."""
    try:
        with open(FILE_NAME, "w") as file:
            json.dump(students, file, indent=4)
    except OSError:
        print("Error: Could not save student data.")


def find_student(roll):
    """Linear search using roll number."""
    for student in students:
        if student["roll"] == roll:
            return student
    return None


def add_student():
    try:
        roll = int(input("Enter roll number: "))

        if roll <= 0:
            print("Roll number must be positive.")
            return

        if find_student(roll) is not None:
            print("Roll number already exists!")
            return

        name = input("Enter student name: ").strip()
        course = input("Enter course: ").strip()
        marks = float(input("Enter marks (0-100): "))

        if not name or not course:
            print("Name and course cannot be empty.")
            return

        if not 0 <= marks <= 100:
            print("Marks must be between 0 and 100.")
            return

        student = {
            "roll": roll,
            "name": name,
            "course": course,
            "marks": marks
        }

        students.append(student)
        save_students()
        print("Student added successfully!")

    except ValueError:
        print("Please enter valid numeric values.")


def display_students():
    if not students:
        print("No student records found.")
        return

    print("\n--- STUDENT RECORDS ---")
    print("-" * 65)
    print(f"{'Roll':<10}{'Name':<22}{'Course':<18}{'Marks':<10}")
    print("-" * 65)

    for student in students:
        print(
            f"{student['roll']:<10}"
            f"{student['name'][:20]:<22}"
            f"{student['course'][:16]:<18}"
            f"{student['marks']:<10.2f}"
        )


def search_student():
    try:
        roll = int(input("Enter roll number to search: "))
        student = find_student(roll)

        if student:
            print("\nStudent Found!")
            print("Roll:", student["roll"])
            print("Name:", student["name"])
            print("Course:", student["course"])
            print("Marks:", student["marks"])
        else:
            print("Student not found.")

    except ValueError:
        print("Enter a valid roll number.")


def update_student():
    try:
        roll = int(input("Enter roll number to update: "))
        student = find_student(roll)

        if student is None:
            print("Student not found.")
            return

        name = input("Enter new name (blank to keep old): ").strip()
        course = input("Enter new course (blank to keep old): ").strip()
        marks_input = input("Enter new marks (blank to keep old): ").strip()

        if name:
            student["name"] = name

        if course:
            student["course"] = course

        if marks_input:
            marks = float(marks_input)

            if not 0 <= marks <= 100:
                print("Marks must be between 0 and 100.")
                return

            student["marks"] = marks

        save_students()
        print("Student updated successfully!")

    except ValueError:
        print("Invalid input. Please enter valid marks or roll number.")


def delete_student():
    try:
        roll = int(input("Enter roll number to delete: "))
        student = find_student(roll)

        if student is None:
            print("Student not found.")
            return

        students.remove(student)
        save_students()
        print("Student deleted successfully!")

    except ValueError:
        print("Enter a valid roll number.")


def sort_students():
    students.sort(key=lambda student: student["marks"], reverse=True)
    save_students()
    print("Students sorted by marks (highest first).")
    display_students()


def main():
    while True:
        print("\n===== STUDENT MANAGEMENT SYSTEM =====")
        print("1. Add Student")
        print("2. Display Students")
        print("3. Search Student")
        print("4. Update Student")
        print("5. Delete Student")
        print("6. Sort by Marks")
        print("7. Exit")

        choice = input("Enter your choice: ").strip()

        if choice == "1":
            add_student()
        elif choice == "2":
            display_students()
        elif choice == "3":
            search_student()
        elif choice == "4":
            update_student()
        elif choice == "5":
            delete_student()
        elif choice == "6":
            sort_students()
        elif choice == "7":
            save_students()
            print("Thank you for using the system!")
            break
        else:
            print("Invalid choice! Select 1-7.")


if __name__ == "__main__":
    main()
