#include <iostream>
#include <fstream>
#include <string>
#include <sstream>  
using namespace std;

class temp
{
private:  
    string userName,
        Email,
        password;
    string searchName, searchPass, searchEmail;
    fstream file;

public:
    void login();
    void signUP();
    void forgot();
};

int main()
{
    char choice;
    while (true)
    {  
        cout << "\n1- Login";
        cout << "\n2- Sign-Up";
        cout << "\n3- Forgot Password";
        cout << "\n4- Exit";
        cout << "\nEnter Your Choice :: ";
        cin >> choice;
        cin.ignore(); 

        switch (choice)
        {
        case '1':
        {
            temp obj;  
            obj.login();
            break;
        }
        case '2':
        {
            temp obj; 
            break;
        }
        case '3':
        {
            temp obj;  
            obj.forgot();
            break;
        }
        case '4':
            cout << "Exiting program..." << endl;
            return 0;
        default:
            cout << "Invalid Selection...!";
        }
    }
    return 0;
}

void temp::signUP()
{
    cout << "\nEnter Your User Name :: ";
    getline(cin, userName);
    cout << "\nEnter Your Email Address :: ";
    getline(cin, Email);
    cout << "\nEnter Your Password :: ";
    getline(cin, password);

    file.open("loginData.txt", ios::out | ios::app);
    if (file.is_open())
    {
        file << userName << "*" << Email << "*" << password << endl;
        file.close();
        cout << "Registration successful!" << endl;
    }
    else
    {
        cerr << "Error opening file for writing!" << endl;
    }
}

void temp::login()
{
    cout << "----------LOGIN---------" << endl;
    cout << "Enter Your User Name :: ";
    getline(cin, searchName);
    cout << "Enter Your Password :: ";
    getline(cin, searchPass);

    file.open("loginData.txt", ios::in);
    if (file.is_open())
    {
        string line;
        while (getline(file, line))
        {
            stringstream ss(line);
            string name, email, pass;

            getline(ss, name, '*');
            getline(ss, email, '*');
            getline(ss, pass, '*');

            if (name == searchName && pass == searchPass)
            {
                cout << "\nAccount Login Successful...!" << endl;
                cout << "Username :: " << name << endl;
                cout << "Email :: " << email << endl;
                file.close();
                return;
            }
        }
        cout << "Invalid username or password!" << endl;
        file.close();
    }
    else
    {
        cerr << "Error opening file for reading!" << endl;
    }
}

void temp::forgot()
{
    cout << "\nEnter Your UserName :: ";
    getline(cin, searchName);
    cout << "\nEnter Your Email Address :: ";
    getline(cin, searchEmail);

    file.open("loginData.txt", ios::in);
    if (file.is_open())
    {
        string line;
        bool found = false;
        while (getline(file, line))
        {
            stringstream ss(line);
            string name, email, pass;

            getline(ss, name, '*');
            getline(ss, email, '*');
            getline(ss, pass, '*');

            if (name == searchName && email == searchEmail)
            {
                cout << "\nAccount found...!" << endl;
                cout << "Your Password :: " << pass << endl;
                found = true;
                break;
            }
        }
        if (!found)
        {
            cout << "User not found with provided username and email." << endl;
        }
        file.close();
    }
    else
    {
        cerr << "Error opening file for reading!" << endl;
    }
}
