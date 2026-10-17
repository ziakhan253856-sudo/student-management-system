#include <iostream>
#include <fstream>
#include <string>
using namespace std;

struct Student {
    int id;
    string name;
    float gpa;
};

void addStudent() {
    Student s;
    cout << "Enter ID: "; cin >> s.id;
    cout << "Enter Name: "; cin >> s.name;
    cout << "Enter GPA: "; cin >> s.gpa;
    ofstream file("students.txt", ios::app);
    file << s.id << " " << s.name << " " << s.gpa << endl;
    file.close();
    cout << "Student Added!\n";
}

void displayStudents() {
    Student s;
    ifstream file("students.txt");
    if (!file) { cout << "No records found!\n"; return; }
    cout << "\n--- Student Records ---\n";
    while (file >> s.id >> s.name >> s.gpa) {
        cout << "ID: " << s.id << " | Name: " << s.name << " | GPA: " << s.gpa << endl;
    }
    file.close();
}

void searchStudent() {
    int searchId;
    cout << "Enter ID to Search: ";
    cin >> searchId;
    Student s;
    ifstream file("students.txt");
    bool found = false;
    while (file >> s.id >> s.name >> s.gpa) {
        if (s.id == searchId) {
            cout << "FOUND -> ID: " << s.id << " | Name: " << s.name << " | GPA: " << s.gpa << endl;
            found = true; break;
        }
    }
    if (!found) cout << "Student Not Found!\n";
    file.close();
}

void deleteStudent() {
    int delId;
    cout << "Enter ID to Delete: ";
    cin >> delId;
    Student s;
    ifstream inFile("students.txt");
    ofstream outFile("temp.txt");
    bool found = false;
    while (inFile >> s.id >> s.name >> s.gpa) {
        if (s.id != delId) outFile << s.id << " " << s.name << " " << s.gpa << endl;
        else found = true;
    }
    inFile.close(); outFile.close();
    remove("students.txt");
    rename("temp.txt", "students.txt");
    if (found) cout << "Student Deleted Successfully!\n";
    else cout << "ID Not Found!\n";
}

int main() {
    int choice;
    do {
        cout << "\n1. Add Student\n2. Display All\n3. Search Student\n4. Delete Student\n5. Exit\nEnter Choice: ";
        cin >> choice;
        if (choice == 1) addStudent();
        else if (choice == 2) displayStudents();
        else if (choice == 3) searchStudent();
        else if (choice == 4) deleteStudent();
    } while (choice != 5);
    return 0;
}