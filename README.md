/**********************************************************************************************
 *  Project Developer : Nofal Ali
 *  Department        : Institute of Computing
 *  Discipline        : Software Engineering
 *  Instructor        : Dr Amjid Mehmood
 * 
 *  -------------------------------------------------------------------------------------------
 *  Project Title     : Student Management System
 *  Language          : C++
 *  File Name         : StudentManagementSystem.cpp
 *
 *  Description       :
 *  -------------------------------------------------------------------------------------------
 *  This is a fully commented, menu-driven Student Management System written in C++.
 *  The program allows the user to:
 *
 *      - Add new students
 *      - Display student records
 *      - Search students by ID or Name
 *      - Update student information
 *      - Delete student records
 *      - Sort students by average or name
 *      - Find top-performing student
 *      - Save student data to file
 *      - Load student data from file
 *
 *  This single-file version is intentionally expanded with:
 *       Detailed comments
 *       Section headers
 *       Explanatory notes
 *       Professional formatting
 *
 *  Total Lines (Final) : 1500+ lines
 *
 *  Author        : Nofal Khattak
 *  Created For  : University / College Project
 *
 **********************************************************************************************/

#include <iostream>     // For input/output operations
#include <string>       // For string data type
#include <limits>       // For numeric_limits
#include <fstream>      // For file handling

using namespace std;

/**********************************************************************************************
 *                                   GLOBAL CONSTANTS
 **********************************************************************************************/

// Total number of subjects offered

const int SUBJECTS = 6;

// Subject names array (used throughout the program)
string subjectNames[SUBJECTS] =
{
    "English",
    "Quantitative Reasoning",
    "ICT",
    "Discrete Mathematics",
    "Islamiat",
    "Programming Fundamentals"
};

/**********************************************************************************************
 *                                   STUDENT STRUCTURE
 **********************************************************************************************/

/*
    Structure Name : Student

    Purpose:
   
 --------
   
 This structure stores all information related to a single student.

    Members:
    --------
    id       → Unique student ID
    name     → Student full name
    marks[]  → Marks obtained in each subject
    total    → Total marks
    average  → Average marks
*/

/* Sorting Functions */
void swapStudents(Student& a, Student& b);
void sortByAverageDescending(Student* arr, int n);
void sortByAverageAscending(Student* arr, int n);
void sortByName(Student* arr, int n);


/* File Handling */
void saveToFile(Student* arr, int n, const string & filename);
Student*loadFromFile(int& n, const string & filename);

/* Utility */
int findTopStudent(Student* arr, int n);

/**********************************************************************************************
 *                                   END OF PART 1
 **********************************************************************************************/
/**********************************************************************************************
 *                               INPUT VALIDATION FUNCTIONS
 **********************************************************************************************/

/*
    Function Name : input ValidatedID

    Purpose:
    --------
    Safely inputs a student ID from the user.
    Ensures:
        - ID is an integer
        - ID is positive
        - No invalid characters are entered

    Returns:
    --------
    int → Validated student ID
*/
int inputValidatedID()
{
    int id;

    while (true)
    {
        cout << "Enter Student ID (positive integer): ";
        cin >> id;

        // Check for invalid input
        if (cin.fail() || id <= 0)
        {
            cin.clear(); // clear error flags
            cin.ignorenumeric_limitsstream size::max(), '\n';
            cout << " Invalid ID. Please enter a positive number.\n";
        }
        else
        {
            cin.ignore(numeric_limits<streamsize>::max(), '\n');
            return id;
        }
    }
}

/*
   
 Function Name : inputValidatedMarks

    Purpose:
    --------
    Inputs subject marks with validation.
    Ensures:
        - Marks are numeric
        - Marks are between 0 and 100

    Returns:
    --------
    float → Validated marks
*/
float inputValidatedMarks()
{
    float marks;

    while (true)
    {
        cin >> marks;

        // Validate marks range
        if (cin.fail() || marks < 0 || marks > 100)
        {
            cin.clear(); // reset error state
            cin.ignore(numeric_limits<streamsize>::max(), '\n');
            cout << " Invalid marks! Enter marks between 0 and 100: ";
        }
        else
        {
           
 return marks;
        }
    }
}

/**********************************************************************************************
 *                               CALCULATION FUNCTIONS
 **********************************************************************************************/

/*
    Function Name : calculateTotal

 * 
    Purpose:
    --------
    Calculates the total marks obtained by a student.

    Parameters:
    -----------
    arr[] → Array containing marks of all subjects
    size  → Number of subjects

    Returns:
    --------
    float → Total marks
*/
float calculateTotal(float arr[], int size)
{
    float sum = 0.0f;

    // Loop through each subject and add marks
    for (int i = 0; i < size; i++)
    {
        sum += arr[i];
    }

    return sum;
}

/*
   

 Function Name : calculateAverage

    Purpose:
    --------
    Calculates average marks of a student.

    Parameters:
    -----------
    arr[] → Array containing marks
    size  → Number of subjects

    Returns:
    --------
    float → Average marks
*/
float calculateAverage(float arr[], int size)

{
    // Prevent division by zero
    
    if (size == 0)
        return 0.0f;

    return calculateTotal(arr, size) / size;
}

/**********************************************************************************************
 *                               STUDENT INPUT FUNCTION
 **********************************************************************************************/

/*
    Function Name : inputStudent

    Purpose:
    --------
    Takes complete student data from the user including:
        - Student ID
        - Student Name
        - Marks for each subject

    Parameters:
    -----------
    s → Pointer to a Student structure
*/
void inputStudent(Student* s)
{
    cout << "\n================ ENTER STUDENT DETAILS ================\n";

    // Input student ID with validation
    s->id = inputValidatedID();

    // Input student name
    cout << "Enter Student Name: ";
    getline(cin, s->name);

    cout << "\nEnter marks for each subject (0 - 100)\n";

    // Input marks for each subject
    for (int i = 0; i < SUBJECTS; i++)
    {
        cout << subjectNames[i] << ": ";
        s->marks[i] = inputValidatedMarks();
    }

    // Calculate total and average
    s->total = calculateTotal(s->marks, SUBJECTS);
    s->average = calculateAverage(s->marks, SUBJECTS);

    cout << " Student data entered successfully.\n";
}

/**********************************************************************************************
 *                               STUDENT DISPLAY FUNCTION
 **********************************************************************************************/

/*
    Function Name : displayStudent

    Purpose:
    --------
     * 
    Displays all details of a student in a formatted manner.

    Parameters:
    -----------
    s → Pointer to constant Student structure
*/
void displayStudent(const Student* s)
{
    cout << "\n======================================================\n";
    cout << "Student ID   : " << s->id << endl;
    cout << "Student Name : " << s->name << endl;
    cout << "\nMarks Obtained:\n";

    // Display subject-wise marks
    
    for (int i = 0; i < SUBJECTS; i++)
   
 {
        cout << "  " << subjectNames[i] << " : " << s->marks[i] << endl;
    }

    cout << "\nTotal Marks  : " << s->total << endl;
    cout << "Average     : " << s->average << endl;

    // Grade calculation based on average
    
    if (s->average >= 90)
        cout << "Grade       : A+\n";
    else if (s->average >= 80)
        cout << "Grade       : A\n";
    else if (s->average >= 70)
        cout << "Grade       : B\n";
    else if (s->average >= 60)
        cout << "Grade       : C\n";
    else
        cout << "Grade       : F\n";

   
 cout << "======================================================\n";
}


/**********************************************************************************************
 *                                   END OF PART 2

 **********************************************************************************************/

/**********************************************************************************************
 *                               SEARCH FUNCTIONS
 **********************************************************************************************/

/*
    Function Name : searchByID

    Purpose:
    --------
    Searches for a student by their unique ID.

    Parameters:
    -----------
    arr[] → Array of Student structures
    n     → Number of students
    id    → ID to search for

    Returns:
    --------
    int → Index of student in array if found, -1 if not found
*/
int searchByID(Student* arr, int n, int id)
{
    // Loop through all students
    for (int i = 0; i < n; i++)
    {
        if (arr[i].id == id)
        {
            return i; // Student found
        }
    }

    return -1; // Student not found
}

/*
    Function Name : searchByName

    Purpose:
    --------
    Searches for a student by their full name.

    Parameters:
    -----------
    arr[] → Array of Student structures
    n     → Number of students
    name  → Name to search for (case-sensitive)

    Returns:
    --------
    int → Index of student in array if found, -1 if not found
*/
int searchByName(Student* arr, int n, string name)
{
    for (int i = 0; i < n; i++)
    {
        if (arr[i].name == name)
        {
            return i; // Student found
        }
    }

    return -1; // Student not found
}

/**********************************************************************************************
 *                               UPDATE STUDENT FUNCTIONS
 **********************************************************************************************/

/*
    Function Name : updateStudent

   
 Purpose:

    --------

    Updates a student's full information (name + marks).

    Parameters:
    -----------
    s → Pointer to Student structure to update
*/
void updateStudent(Student* s)
{
    cout << "\n================ UPDATE STUDENT ======================\n";
    cout << "Updating Student ID: " << s->id << endl;

    // Input new name
    cout << "Enter new student name: ";
    getline(cin, s->name);

    // Input new marks
    cout << "\nEnter new marks for each subject (0 - 100)\n";
    for (int i = 0; i < SUBJECTS; i++)
    {
        cout << subjectNames[i] << ": ";
        s->marks[i] = inputValidatedMarks();
    }

    // Recalculate total and average
    s->total = calculateTotal(s->marks, SUBJECTS);
    s->average = calculateAverage(s->marks, SUBJECTS);

    cout << " Student updated successfully.\n";
}

/*
    Function Name : updateStudentMarks

    Purpose:
    --------
    Updates only the marks of a student.

    Parameters:
    -----------
    s → Pointer to Student structure to update
*/
void updateStudentMarks(Student* s)
{
    cout << "\n================ UPDATE STUDENT MARKS =================\n";
    cout << "Updating marks for Student ID: " << s->id << endl;

    for (int i = 0; i < SUBJECTS; i++)
    {
        cout << "Enter new marks for " << subjectNames[i] << ": ";
        s->marks[i] = inputValidatedMarks();
    }

    // Recalculate total and average
    s->total = calculateTotal(s->marks, SUBJECTS);
    s->average = calculateAverage(s->marks, SUBJECTS);

    cout << " Marks updated successfully.\n";
}

/**********************************************************************************************
 *                               DELETE STUDENT FUNCTION
 **********************************************************************************************/

/*
    Function Name : deleteStudent

    Purpose:
    --------
    Deletes a student from the array by ID.
    After deletion, array size is reduced by 1.
    Memory is reallocated to avoid unused memory.

    Parameters:
    -----------
    arr → Reference to array of Student structures
    n   → Reference to number of students
    id  → ID of student to delete

    Returns:
    --------
    bool → true if deleted successfully, false if student not found
*/
bool deleteStudent(Student*& arr, int& n, int id)
{
    int index = searchByID(arr, n, id);

    if (index == -1)
    {
        cout << " Student with ID " << id << " not found.\n";
        return false;
    }

    cout << "Deleting Student ID: " << id << " (" << arr[index].name << ")\n";

    // Shift all students after index to left
    for (int i = index; i < n - 1; i++)
    {
        arr[i] = arr[i + 1];
    }

    n--; // Reduce total number of students

    // Reallocate memory to avoid unused space
    Student* temp = new Student[n];
    for (int i = 0; i < n; i++)
    {
        temp[i] = arr[i];
    }

    delete[] arr; // Free old array
    arr = temp;

    cout << " Student deleted successfully.\n";
    return true;
}

/**********************************************************************************************
 *                               FIND TOP STUDENT FUNCTION
 **********************************************************************************************/

/*
    Function Name : findTopStudent

    Purpose:
    --------
    Finds the student with the highest average.

    Parameters:
    -----------
    arr[] → Array of Student structures
    n     → Number of students

    Returns:
    --------
    int → Index of top student
*/
int findTopStudent(Student* arr, int n)
{
    if (n == 0)
       
 return -1;

    int topIndex = 0;

    for (int i = 1; i < n; i++)
    {
        if (arr[i].average > arr[topIndex].average)
        {
            topIndex = i;
        }
    }

    return topIndex;
}

/**********************************************************************************************
 *                                   END OF PART 3
 **********************************************************************************************/
/**********************************************************************************************
 *                                   SORTING FUNCTIONS
 **********************************************************************************************/

/*
    Function Name : swapStudents

    Purpose:
    --------
    Swaps the data of two Student structures.

    Parameters:
    -----------
    a → Reference to first Student
     * 
    b → Reference to second Student
*/
void swapStudents(Student& a, Student& b)
{
    Student temp = a;
    a = b;
    b = temp;
}

/*
    Function Name : sortByAverageDescending

    Purpose:
    --------
    Sorts the array of students by average marks in descending order
    (highest average first) using Bubble Sort.

    Parameters:
    -----------
    arr → Array of Student structures
    n   → Number of students
*/
void sortByAverageDescending(Student* arr, int n)
{
    if (n <= 1)
    {
        cout << " Not enough students to sort.\n";
        return;
    }

    for (int i = 0; i < n - 1; i++)
    {
        for (int j = 0; j < n - i - 1; j++)
        {
            if (arr[j].average < arr[j + 1].average)
            {
                swapStudents(arr[j], arr[j + 1]);
            }
        }
    }

    cout << " Students sorted by average (Highest → Lowest).\n";
}

/*
    Function Name : sortByAverageAscending

    Purpose:
    --------
    Sorts the array of students by average marks in ascending order
    (lowest average first) using Bubble Sort.

    Parameters:
    -----------
    arr → Array of Student structures
    n   → Number of students
*/
void sortByAverageAscending(Student* arr, int n)
{
    if (n <= 1)
    {
        cout << " Not enough students to sort.\n";
        return;
    }

    for (int i = 0; i < n - 1; i++)
    {
        for (int j = 0; j < n - i - 1; j++)
        {
            if (arr[j].average > arr[j + 1].average)
            {
                swapStudents(arr[j], arr[j + 1]);
            }
        }
    }

    cout << " Students sorted by average (Lowest → Highest).\n";
}

/*
    Function Name : sortByName

    Purpose:
    --------
    Sorts students alphabetically by name (A → Z) using Bubble Sort.

    Parameters:
    -----------
    arr → Array of Student structures
    n   → Number of students
*/
void sortByName(Student* arr, int n)
{
    if (n <= 1)
    {
        cout << " Not enough students to sort.\n";
        return;
    }

    for (int i = 0; i < n - 1; i++)
    {
        for (int j = 0; j < n - i - 1; j++)
        {
            if (arr[j].name > arr[j + 1].name)
            {
                swapStudents(arr[j], arr[j + 1]);
            }
        }
    }

    cout << " Students sorted alphabetically (A → Z).\n";
}

/**********************************************************************************************
 *                                   FILE HANDLING FUNCTIONS
 **********************************************************************************************/

/*
    Function Name : saveToFile

    Purpose:
    --------
    Saves all student data to a file. File format:

    [Number of Students]
    ID
    Name
    Marks (space-separated)
    Total Average

    Parameters:
    -----------
    arr      → Array of Student structures
    n        → Number of students
    filename → Name of file to save
*/
void saveToFile(Student* arr, int n, const string& filename)
{
    ofstream fout(filename);

    if (!fout)
    {
        cout << " Error opening file for writing!\n";
        return;
    }

    fout << n << endl;

    for (int i = 0; i < n; i++)
    {
        fout << arr[i].id << endl;
        fout << arr[i].name << endl;

        for (int j = 0; j < SUBJECTS; j++)
        {
            fout << arr[i].marks[j] << " ";
        }

        fout << endl;
        fout << arr[i].total << " " << arr[i].average << endl;
    }

    fout.close();

    cout << "✔ Student data saved successfully to \"" << filename << "\"\n";
}

/*
    Function Name : loadFromFile

    Purpose:
    --------
    Loads student data from a file. If file does not exist, returns nullptr.

    File format expected:
    ---------------------
    [Number of Students]
    ID
    Name
    Marks (space-separated)
    Total Average

    Parameters:
    -----------
    n        → Reference to store number of students
    filename → File to load

    Returns:
    --------
    Student* → Dynamically allocated array of students (must delete[] later)
*/
Student* loadFromFile(int& n, const string& filename)
{
    ifstream fin(filename);

    if (!fin)
    {
        cout << " File not found or cannot be opened!\n";
        n = 0;
        return nullptr;
    }

    fin >> n;
    fin.ignore(numeric_limits<streamsize>::max(), '\n'); // Ignore newline

    Student* arr = new Student[n];

    for (int i = 0; i < n; i++)
    {
        fin >> arr[i].id;
        fin.ignore(numeric_limits<streamsize>::max(), '\n'); // Ignore newline

        getline(fin, arr[i].name);

        for (int j = 0; j < SUBJECTS; j++)
        {
            fin >> arr[i].marks[j];
        }

        fin >> arr[i].total >> arr[i].average;
        fin.ignore(numeric_limits<streamsize>::max(), '\n'); // Ignore newline
    }

    fin.close();

    cout << "✔ Student data loaded successfully from \"" << filename << "\"\n";

    return arr;
}

/**********************************************************************************************
 *                                   END OF PART 4
 **********************************************************************************************/
/**********************************************************************************************
 *                                   MAIN MENU FUNCTIONS
 **********************************************************************************************/

/*
    Function Name : displayMenu

    Purpose:
    --------
    Displays all available menu options to the user.
*/
void displayMenu()
{
    cout << "\n================ STUDENT MANAGEMENT SYSTEM ================\n";
    cout << "1. Load Students from File\n";
    cout << "2. Enter New Students\n";
    cout << "3. Display All Students\n";
    cout << "4. Search by ID\n";
    cout << "5. Search by Name\n";
    cout << "6. Update Student (Name + Marks)\n";
    cout << "7. Update Student Marks Only\n";
    cout << "8. Delete Student\n";
    cout << "9. Show Top Student\n";
    cout << "10. Save Students to File\n";
    cout << "11. Sort by Average (Highest → Lowest)\n";
    cout << "12. Sort by Average (Lowest → Highest)\n";
    cout << "13. Sort by Name (A → Z)\n";
    cout << "14. Exit\n";
    cout << "Enter your choice: ";
}

/**********************************************************************************************
 *                                   MAIN FUNCTION
 **********************************************************************************************/

int main()
{
    int n = 0;               // Number of students
    Student* students = nullptr;  // Dynamic array for students
    int choice;

    do
    {
        displayMenu();
        cin >> choice;

        // Clear input buffer to prevent errors for string input
        cin.ignore(numeric_limits<streamsize>::max(), '\n');

        switch (choice)
        {
            case 1:  // Load from file
            {
                string filename;
                cout << "Enter filename to load: ";
                cin >> filename;

                // Delete existing students array
                delete[] students;

                students = loadFromFile(n, filename);
                break;
            }

            case 2:  // Enter new students
            {
                int newCount;
                cout << "Enter number of students: ";
                cin >> newCount;
                cin.ignore(numeric_limits<streamsize>::max(), '\n');

                delete[] students;       // Clear old array
                n = newCount;
                students = new Student[n];

                for (int i = 0; i < n; i++)
                {
                    cout << "\n--- Enter details for student " << i + 1 << " ---\n";
                    inputStudent(&students[i]);
                }
                break;
            }

            case 3:  // Display all students
            {
                if (n == 0 || students == nullptr)
                {
                    cout << " No students to display.\n";
                }
                else
                {
                    for (int i = 0; i < n; i++)
                    {
                        displayStudent(&students[i]);
                    }
                }
                break;
            }

            case 4:  // Search by ID
            {
                if (n == 0 || students == nullptr)
                {
                    cout << " No students available.\n";
                    break;
                }

                int id;
                cout << "Enter ID to search: ";
                cin >> id;
                int index = searchByID(students, n, id);

                if (index != -1)
                    displayStudent(&students[index]);
                else
                    cout << " Student not found.\n";

                break;
            }

            case 5:  // Search by Name
            {
                if (n == 0 || students == nullptr)
                {
                    cout << " No students available.\n";
                    break;
                }

                string name;
                cout << "Enter Name to search: ";
                getline(cin, name);
                int index = searchByName(students, n, name);

                if (index != -1)
                    displayStudent(&students[index]);
                else
                    cout << " Student not found.\n";

                break;
            }

            case 6:  // Update full student data
            {
                if (n == 0 || students == nullptr)
                {
                    cout << " No students available.\n";
                    break;
                }

                int id;
                cout << "Enter ID to update: ";
                cin >> id;
                cin.ignore(numeric_limits<streamsize>::max(), '\n');

                int index = searchByID(students, n, id);
                if (index != -1)
                    updateStudent(&students[index]);
                else
                    cout << " Student not found.\n";

                break;
            }

            case 7:  // Update only marks
            {
                if (n == 0 || students == nullptr)
                {
                    cout << " No students available.\n";
                    break;
                }

                int id;
                cout << "Enter ID to update marks: ";
                cin >> id;
                cin.ignore(numeric_limits<streamsize>::max(), '\n');

                int index = searchByID(students, n, id);
                if (index != -1)
                    updateStudentMarks(&students[index]);
                else
                    cout << " Student not found.\n";

                break;
            }

            case 8:  // Delete student
            {
                if (n == 0 || students == nullptr)
                {
                    cout << " No students available.\n";
                    break;
                }

                int id;
                cout << "Enter ID to delete: ";
                cin >> id;
                cin.ignore(numeric_limits<streamsize>::max(), '\n');

                deleteStudent(students, n, id);
                break;
            }

            case 9:  // Show top student
            {
                if (n == 0 || students == nullptr)
                {
                    cout << " No students available.\n";
                    break;
                }

                int topIndex = findTopStudent(students, n);
                if (topIndex != -1)
                {
                    cout << "\n Top Student:\n";
                    displayStudent(&students[topIndex]);
                }
                break;
            }

            case 10: // Save students to file
            {
                if (n == 0 || students == nullptr)
                {
                    cout << " No students to save.\n";
                    break;
                }

                string filename;
                cout << "Enter filename to save: ";
                cin >> filename;

                saveToFile(students, n, filename);
                break;
            }

            case 11: // Sort by average descending
            {
                if (n == 0 || students == nullptr)
                {
                    cout << " No students to sort.\n";
                    break;
                }

                sortByAverageDescending(students, n);
                break;
            }

            case 12: // Sort by average ascending
            {
                if (n == 0 || students == nullptr)
                {
                    cout << " No students to sort.\n";
                    break;
                }

                sortByAverageAscending(students, n);
                break;
            }

            case 13: // Sort by name A → Z
            {
                if (n == 0 || students == nullptr)
                {
                    cout << " No students to sort.\n";
                    break;
                }

                sortByName(students, n);
                break;
            }

            case 14: // Exit
                cout << "Exiting program. Goodbye! \n";
                break;

            default:
                cout << " Invalid choice. Please try again.\n";
        }

    } while (choice != 14);

    // Free dynamic memory
    delete[] students;
    students = nullptr;

    cout << "\n Program terminated successfully.\n";
    return 0;
}struct Student
{
    int id;                     // Student ID
    string name;                // Student Name
    float marks[SUBJECTS];      // Marks for each subject
    float total;                // Total marks
    float average;              // Average marks
};

/**********************************************************************************************
 *                          FUNCTION DECLARATIONS (PROTOTYPES)
 **********************************************************************************************/

/* Calculation Functions */
float calculateTotal(float arr[], int size);
float calculateAverage(float arr[], int size);


/* Validation Functions */
float inputValidatedMarks();
int inputValidatedID();


/* Student Input / Output */
void inputStudent(Student* s);
void displayStudent(const Student* s);

 
/* Search Functions */
int searchByID(Student* arr, int n, int id);
int searchByName(Student* arr, int n, string name); 


/* Update Functions */
void updateStudent(Student* s);
void updatestudentmarks(Student* s);

{
/* Delete Function */
bool deleteStudent(Student*& arr, int& n, int id);

return 0;

}
