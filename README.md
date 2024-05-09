#include <iostream>
#include <string>
#include <queue>

using namespace std;

struct User {
    string username;
    int userID;
    int age;
    int daysOfWorking; 

    User() {}
    User(const string& name, int id, int userAge, int workingDays) : username(name), userID(id), age(userAge), daysOfWorking(workingDays) {}
};

struct Node {
    User data;
    Node* next;

    Node(const User& userData) : data(userData), next(nullptr) {}
};

class UserDatabase {
private:
    Node* head;
    queue<User> userQueue;
    User users[1];
    int userCount;

public:
    UserDatabase() : head(nullptr), userCount(0) {}

    void createUser(const string& name, int age, int daysOfWorking) {
        User newUser(name, userCount + 1, age, daysOfWorking);
        users[userCount] = newUser;
        userCount++;

        Node* newNode = new Node(newUser);
        if (!head) {
            head = newNode;
        } else {
            Node* temp = head;
            while (temp->next) {
                temp = temp->next;
            }
            temp->next = newNode;
        }
        userQueue.push(newUser);

        cout << "User " << newUser.username << " created successfully!" << endl;
    }

    void readUser(int userID) {
        if (userID >= 1 && userID <= userCount) {
            User user = users[userID - 1];
            cout << "User ID: " << user.userID << endl;
            cout << "Username: " << user.username << endl;
            cout << "Age: " << user.age << endl; // Display age
            cout << "Days of Working: " << user.daysOfWorking << endl; // Display days of working
        } else {
            cout << "User ID not found!" << endl;
        }
    }

    void updateUser(int userID, const string& newName, int newAge, int newDaysOfWorking) {
        if (userID >= 1 && userID <= userCount) {
            users[userID - 1].username = newName;
            users[userID - 1].age = newAge; // Update age
            users[userID - 1].daysOfWorking = newDaysOfWorking; // Update days of working
            cout << "User information updated successfully!" << endl;
        } else {
            cout << "User ID not found!" << endl;
        }
    }

    void deleteUser(int userID) {
        if (userID >= 1 && userID <= userCount) {
            for (int i = userID - 1; i < userCount - 1; ++i) {
                users[i] = users[i + 1];
            }
            userCount--;

            cout << "User deleted successfully!" << endl;
        } else {
            cout << "User ID not found!" << endl;
        }
    }

    void displayUsers() {
        if (userCount == 0) {
            cout << "User not found!" << endl;
        } else {
            cout << "User found!" << endl;
            for (int i = 0; i < userCount; ++i) {
                cout << "User ID: " << users[i].userID << ", Username: " << users[i].username << ", Age: " << users[i].age << ", Days of Working: " << users[i].daysOfWorking << endl;
            }
        }
    }
};

int main() {
    UserDatabase database;

    int choice;
    do {
        cout <<"  ============================================================================"<<endl;
        cout <<"                            Organizational Structure"                          <<endl;
        cout <<"  ============================================================================"<<endl;
        cout << "\n    1. Create User                                      4. Delete User      ";
        cout << "\n    2. Read User                                        5. Display User     ";
        cout << "\n    3. Update User                                      6. Exit             "<<endl;
        cout <<"  ============================================================================"<<endl;
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1: {
                string name;
                int age;
                int daysOfWorking;
                cout << "Enter username: ";
                cin >> name;
                cout << "Enter age: ";
                cin >> age;
                cout << "Enter days of working: ";
                cin >> daysOfWorking;
                database.createUser(name, age, daysOfWorking);
                break;
            }
            case 2: {
                int userID;
                cout << "Enter user ID: ";
                cin >> userID;
                database.readUser(userID);
                break;
            }
            case 3: {
                int userID;
                string newName;
                int newAge;
                int newDaysOfWorking;
                cout << "Enter user ID: ";
                cin >> userID;
                cout << "Enter new username: ";
                cin >> newName;
                cout << "Enter new age: ";
                cin >> newAge;
                cout << "Enter new days of working: ";
                cin >> newDaysOfWorking;
                database.updateUser(userID, newName, newAge, newDaysOfWorking);
                break;
            }
            case 4: {
                int userID;
                cout << "Enter user ID to delete: ";
                cin >> userID;
                database.deleteUser(userID);
                break;
            }
            case 5:
                database.displayUsers();
                break;
            case 6:
                cout << "Exiting program..." << endl;
                break;
            default:
                cout << "Invalid choice! Please try again." << endl;
        }
    } while (choice != 6);

    return 0;
}
