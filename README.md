#include <iostream>
#include <string>
using namespace std;

const int MAX_STUDENTS = 10;
const int MAX_SUBJECTS = 5;

struct Subject {
    string name;
    int credit;
    double mark;
    char grade;
};

struct Student {
    string name;
    int age;
    Subject subjects[MAX_SUBJECTS];
    int subjectCount;
    double cpa;
};

void inputSubject(Subject &sub);
void inputStudent(Student &stu);
char calculateGrade(double mark);
double calculateCPA(Student stu);
void displayStudent(const Student &stu);

int main() {
    Student students[MAX_STUDENTS];
    int studentCount = 0;
    char choice;

    cout << "===== STUDENT INFORMATION SYSTEM =====\n";

    do {
        if (studentCount >= MAX_STUDENTS) {
            cout << "Maximum number of students reached!\n";
            break;
        }

        cout << "\nEnter details for student #" << studentCount + 1 << ":\n";
        inputStudent(students[studentCount]);
        studentCount++;

        cout << "\nAdd another student? (y/n): ";
        cin >> choice;
        cin.ignore();

    } while (tolower(choice) == 'y');

    cout << "\n===== DISPLAYING STUDENT DETAILS =====\n";
    for (int i = 0; i < studentCount; i++) {
        cout << "\n--- Student #" << i + 1 << " ---\n";
        displayStudent(students[i]);
    }

    return 0;
}

void inputSubject(Subject &sub) {
    cout << "Enter subject name: ";
    getline(cin, sub.name);

    cout << "Enter credit hour: ";
    cin >> sub.credit;

    do {
        cout << "Enter mark (0 - 100): ";
        cin >> sub.mark;

        if (sub.mark < 0 || sub.mark > 100)
            cout << "❌ Invalid mark! Please enter a value between 0 and 100.\n";

    } while (sub.mark < 0 || sub.mark > 100);

    sub.grade = calculateGrade(sub.mark);

    cin.ignore(); 
}

void inputStudent(Student &stu) {
    cout << "Enter student name: ";
    getline(cin, stu.name);

    cout << "Enter age: ";
    cin >> stu.age;

    cout << "Enter number of subjects (max " << MAX_SUBJECTS << "): ";
    cin >> stu.subjectCount;
    cin.ignore();

    if (stu.subjectCount > MAX_SUBJECTS)
        stu.subjectCount = MAX_SUBJECTS;

    for (int i = 0; i < stu.subjectCount; i++) {
        cout << "\nSubject #" << i + 1 << " details:\n";
        inputSubject(stu.subjects[i]);
    }

    stu.cpa = calculateCPA(stu);
}

char calculateGrade(double mark) {
    if (mark >= 85) return 'A';
    else if (mark >= 70) return 'B';
    else if (mark >= 55) return 'C';
    else if (mark >= 40) return 'D';
    else return 'F';
}

double calculateCPA(Student stu) {
    double totalPoints = 0, totalCredits = 0;

    for (int i = 0; i < stu.subjectCount; i++) {
        double gradePoint;
        switch (stu.subjects[i].grade) {
            case 'A': gradePoint = 4.0; break;
            case 'B': gradePoint = 3.0; break;
            case 'C': gradePoint = 2.0; break;
            case 'D': gradePoint = 1.0; break;
            default:  gradePoint = 0.0; break;
        }

        totalPoints += gradePoint * stu.subjects[i].credit;
        totalCredits += stu.subjects[i].credit;
    }

    if (totalCredits == 0) return 0;
    return totalPoints / totalCredits;
}

void displayStudent(const Student &stu) {
    cout << "Name: " << stu.name << endl;
    cout << "Age: " << stu.age << endl;
    cout << "Subjects:\n";

    for (int i = 0; i < stu.subjectCount; i++) {
        cout << "  " << i + 1 << ". " << stu.subjects[i].name
             << " | Credit: " << stu.subjects[i].credit
             << " | Mark: " << stu.subjects[i].mark
             << " | Grade: " << stu.subjects[i].grade << endl;
    }

    cout << "CPA: " << stu.cpa << "\n";
}
