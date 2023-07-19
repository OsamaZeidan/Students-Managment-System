#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// Structs:
typedef struct Date
{
    int day;
    int month;
    int year;
} Date;

typedef struct Student
{
    int ID;
    char name[20];
    char city[10];
    int classID;
    Date date;

    // BST:
    struct Student *Left;
    struct Student *Right;
} Student;

typedef Student *std;
typedef Student *tree;

// Proto Types of Functions:
std findStd(int id, tree root);
void insertStd(int id, char name[20], char city[10], int classID, Date date, tree *root);
int stdsNumber(tree root);
void treeToArr(tree root, std *arr, int *index);
void treeToArrCity(tree root, std *arr, char city[], int *index);
void sortByname(std *arr, int length);
void sortByClassId(std *arr, int length);
void printStdsByClass(std *arr, int length);
std findMinId(tree root);
int deleteStd(int id, tree *root);
void printMenue(void);
void saveTreeOnFile(tree root);
tree fileToTree(void);
void printStdsArr(std *arr, int length);
int stdsInCity(tree root, char city[]);

int main(void)
{
    tree stds = fileToTree();
    int choice = -1;
    while (1 == 1)
    {
        printMenue();
        printf("\nEnter your choice: ");
        choice = -1;
        while (scanf("%d", &choice) != 1)
        {
            printf("Invalid Input, Please Try Again!");
            printf("\nEnter your choice: ");
            // clear the input buffer...
            while (getchar() != '\n');
        } // end while()

        switch (choice)
        {
        case 1:
        {
            int id = 0;
            char name[20];
            char city[10];
            int classID = 0;
            Date date;
            printf("Enter ID: ");
            while(scanf("%d", &id) != 1)
            {
                printf("Invalid Input, Please Try Again!\n");
                printf("Enter ID: ");
                // clear the input buffer...
                while (getchar() != '\n');
            } // end while()
            if(findStd(id, stds) != NULL)
            {
                printf("\n******************************\nThe Student Already Exists.\n");
                break;
            } // end if()
            printf("Enter Name: ");
            scanf("%s", name);
            printf("Enter City: ");
            scanf("%s", city);
            printf("Enter Class ID: ");
            while(scanf("%d", &classID) != 1)
            {
                printf("Invalid Input, Please Try Again!\n");
                printf("Enter Class ID: ");
                // clear the input buffer...
                while (getchar() != '\n');
            } // end while()
            printf("Enter Date: (DD/MM/YYYY): ");
            scanf("%d/%d/%d", &date.day, &date.month, &date.year);

            while(date.day  <= 0 || date.day > 31 || date.month > 12 || date.month <= 0 || date.year <= 0)
            {
                    printf("Invalid Input, Please Try Again!\n");
                    printf("Enter Date: (DD/MM/YYYY): ");
                    scanf("%d/%d/%d", &date.day, &date.month, &date.year);
            }
            insertStd(id, name, city, classID, date, &stds);
            break;
        } // end case 1
        case 2:
        {
            int id = 0;
            printf("Enter ID: ");
            while(scanf("%d", &id) != 1)
            {
                printf("Invalid Input, Please Try Again!\n");
                printf("Enter ID: ");
                // clear the input buffer...
                while (getchar() != '\n');
            } // end while()
            std std = findStd(id, stds);
            if (std != NULL)
            {
                printf("\nID: %d\t", std->ID);
                printf("Name: %s\t", std->name);
                printf("City: %s\t", std->city);
                printf("Class ID: %d\t", std->classID);
                printf("Date: %d/%d/%d\t", std->date.day, std->date.month, std->date.year);
                printf("\n*************************\nTo Update the Student Info. Enter \"1\".");
                printf("\nTo Update the Student Name Enter \"2\".");
                printf("\nTo Update the Student City Enter \"3\".");
                printf("\nTo Update the Student Class ID Enter \"4\".");
                printf("\nTo Update the Student Enrollment Date Enter \"5\".");
                printf("\nTo Continue Enter \"6\".");
                int x = 0;
                scanf("%d", &x);
                switch (x)
                {

                case 1:
                {
                    int id = 0;
                    char name[20];
                    char city[10];
                    int classID = 0;
                    Date date;
                    printf("Enter ID: ");
                    while(scanf("%d", &id) != 1)
                    {
                        printf("Invalid Input Try Again!\n");
                        scanf("%d", &id);
                         while (getchar() != '\n');
                    } // end while()
                    printf("Enter Name: ");
                    scanf("%s", name);
                    printf("Enter City: ");
                    scanf("%s", city);
                    printf("Enter Class ID: ");
                    scanf("%d", &classID);
                    printf("Enter Date: ");
                    scanf("%d/%d/%d", &date.day, &date.month, &date.year);
                    std->ID = id;
                    strcpy(std->name, name);
                    strcpy(std->city, city);
                    std->classID = classID;
                    std->date = date;
                    break;
                } // end case 1
                case 2:
                {
                    char name[2][20];
                    printf("Enter Name: ");
                    scanf("%s", name);
                    strcpy(std->name, name);
                    break;
                } // end case 2
                case 3:
                {
                    char city[10];
                    printf("Enter City: ");
                    scanf("%s", city);
                    strcpy(std->city, city);
                    break;
                } // end case 3
                case 4:
                {
                    int classID = 0;
                    printf("Enter Class ID: ");
                    scanf("%d", &classID);
                    std->classID = classID;
                    break;
                } // end case 4
                case 5:
                {
                    Date date;
                    date.day = 0;
                    date.month = 0;
                    date.year = 0;
                    printf("Enter Date: ");
                    scanf("%d/%d/%d", &date.day, &date.month, &date.year);
                    std->date = date;
                    break;
                } // end case 5
                case 6:
                {
                    break;
                } // end case 6
                default:
                {
                    printf("Invalid Input!");
                    break;
                } // end default
                } // end switch()

            } // end if()
            else
            {
                printf("\n*****************************************\nStudent Not Found!\n");
            } // end else()
            break;
        } // end case 2
        case 3:
        {
            int num = stdsNumber(stds);
            std *arr = malloc(sizeof(Student) * num);
            int index = 0;
            treeToArr(stds, arr, &index);
            sortByname(arr, stdsNumber(stds));
            printStdsArr(arr, num);
            break;
        } // end case 3
        case 4:
        {

            printf("Enter the City Name:");
            char name[10] = "";
            scanf("%s", name);
            int num1 = stdsInCity(stds, name);
            std *arr = malloc(sizeof(Student) * num1);
            int index1 = 0;
            treeToArrCity(stds, arr, name, &index1);
            printf("%ld\n", sizeof(arr));
            if (index1 != 0)
            {
                sortByname(arr, num1);
                sortByClassId(arr, num1);
                printStdsArr(arr, num1);
            } // end if()
            else
            {
                printf("This City Not Found!\n");
            } // end else()
            break;
        } // end case 4
        case 5:
        {
            int num3 = stdsNumber(stds);
            std *arr = malloc(sizeof(Student) * num3);
            int index2 = 0;
            treeToArr(stds, arr, &index2);
            sortByname(arr, num3);
            sortByClassId(arr, num3);
            printStdsByClass(arr, num3);
            break;
        } // end case 5
        case 6:
        {
            int id1;
            printf("Enter the ID of Student to Delete: ");
            scanf("%d", &id1);
            int deleted = deleteStd(id1, &stds);
            if (deleted)
            {
                printf("\n******************************************************\nStudent Deleted Successfully!\n");
            } // end if()
            else
            {
                printf("\n******************************************************\nStudent Not Found!\n");
            } // end else()
            break;
        } // end case 6
        case 7:
        {
            FILE *out = fopen("students.data", "w");
            fprintf(out, "");
            saveTreeOnFile(stds);
            printf("\n_________Students Saved________________\n");
            break;
        } // end case 7
        case 8:
        {
            printf("Be Sure That You Saved The Changes Before Exit, To Continue Enter \"1\": ");
            int x = 0;
            scanf("%d", &x);
            if (x == 1)
            {
                printf("Good Bye!...\n");
                exit(0);
            }
            else
            {
                break;
            }
        }
        default:
        {
            printf("Invalid Input!\n");
            break;
        } // end default
        } // end switch()
    }     // end while()
} // end main()

/*
    Function Name: insertStd()
    Input: int id, char name[20], char city[10], int classID, Date date, tree *root
    Output: void
    Function Operation: This function inserts a new student in the tree.
*/
void insertStd(int id, char name[20], char city[10], int classID, Date date, tree *root)
{
    if (*root == NULL)
    {
        *root = (tree)malloc(sizeof(Student));
        (*root)->ID = id;
        strcpy((*root)->name, name);
        strcpy((*root)->city, city);
        (*root)->classID = classID;
        (*root)->date = date;
        (*root)->Left = NULL;
        (*root)->Right = NULL;
    } // end if()
    else if (id < (*root)->ID)
    {
        insertStd(id, name, city, classID, date, &((*root)->Left));
    } // end else if()
    else if (id > (*root)->ID)
    {
        insertStd(id, name, city, classID, date, &((*root)->Right));
    } // end else if()
    else
    {
        return;
    } // end if()
} // end of insertStd()

/*
    Function Name: findStd()
    Input: int id, tree root
    Output: std
    Function Operation: This function finds a student in the tree.
*/
std findStd(int id, tree root)
{
    if (root == NULL)
    {
        return NULL;
    } // end if()
    else if (id < root->ID)
    {
        return findStd(id, root->Left);
    } // end else if()
    else if (id > root->ID)
    {
        return findStd(id, root->Right);
    } // end else if()
    else
    {
        return root;
    } // end if()
} // end findStd()

/*
    Function Name: printStdsArr
    Input: std *arr, int length
    Output: void
    Function Operation: This function prints an array of students.
*/
void printStdsArr(std *arr, int length)
{
    for (int i = 0; i < length; i++)
    {
        printf("\n\n******************************************************\nID: %d\t", arr[i]->ID);
        printf("Name: %s\t", arr[i]->name);
        printf("City: %s\t", arr[i]->city);
        printf("Class ID: %d\t", arr[i]->classID);
        printf("Date: %d/%d/%d\t\n", arr[i]->date.day, arr[i]->date.month, arr[i]->date.year);
    } // end for()
} // end printStdsArr()

/*
    Function Name: stdsNumber()
    Input: tree root
    Output: int
    Function Operation: This function returns the number of students in the tree.
*/
int stdsNumber(tree root)
{
    if (root == NULL)
    {
        return 0;
    } // end if()
    else
    {
        return 1 + stdsNumber(root->Left) + stdsNumber(root->Right);
    } // end else()
} // end stdsNumber()

/*
    Function Name: treeToArr()
    Input: tree root, std *arr, int *index
    Output: void
    Function Operation: This function converts a tree to an array.
*/
void treeToArr(tree root, std *arr, int *index)
{
    if (root != NULL)
    {
        treeToArr(root->Left, arr, index);
        arr[*index] = root;
        (*index)++;
        treeToArr(root->Right, arr, index);
    } // end if()
} // end treeToArr()

/*
    Function Name: stdsInCity
    Input: tree root, char city[]
    Output: int
    Function Operation: This function returns the number of students in a city.
*/
int stdsInCity(tree root, char city[])
{
    if (root == NULL)
    {
        return 0;
    } // end if()
    else
    {
        if (strcmp(root->city, city) == 0)
        {
            return 1 + stdsInCity(root->Left, city) + stdsInCity(root->Right, city);
        } // end if()
        else
        {
            return stdsInCity(root->Left, city) + stdsInCity(root->Right, city);
        } // end else()
    }     // end else()
}

/*
    Function Name: treeToArrCity()
    Input: tree root, std *arr, char city[], int *index
    Output: void
    Function Operation: This function converts a tree to an array of students in a city.
*/
void treeToArrCity(tree root, std *arr, char city[], int *index)
{
    if (root != NULL)
    {
        treeToArrCity(root->Left, arr, city, index);
        if (strcmp(root->city, city) == 0)
        {
            arr[*index] = root;
            (*index)++;
        } // end if()
        treeToArrCity(root->Right, arr, city, index);
    } // end if()
} // end treeToArrClass()

/*
    Function Name: sortByname()
    Input: std *arr, int length
    Output: void
    Function Operation: This function sorts an array of students by name.
*/
void sortByname(std *arr, int length)
{
    // Insertion Sort
    std temp;
    int j;
    for (int i = 1; i < length; i++)
    {
        temp = arr[i];
        j = i - 1;
        while (j >= 0 && strcmp(arr[j]->name, temp->name) > 0)
        {
            arr[j + 1] = arr[j];
            j--;
        } // end while()
        arr[j + 1] = temp;
    } // end for()
} // end sortByname()

/*
    Function Name: sortByClassId()
    Input: std *arr, int length
    Output: void
    Function Operation: This function sorts an array of students by class ID.
*/
void sortByClassId(std *arr, int length)
{
    // Insertion Sort
    std temp;
    int j;
    for (int i = 1; i < length; i++)
    {
        temp = arr[i];
        j = i - 1;
        while (j >= 0 && arr[j]->classID > temp->classID)
        {
            arr[j + 1] = arr[j];
            j--;
        } // end while()
        arr[j + 1] = temp;
    } // end for()

} // end sortByClassId()

/*
    Function Name: printStdsByClass()
    Input: std *arr, int length
    Output: void
    Function Operation: This function prints an array of students by class ID.
*/
void printStdsByClass(std *arr, int length)
{
    int currentClass = -1;
    for (int i = 0; i < length; i++)
    {
        if (arr[i]->classID != currentClass)
        {
            printf("\nClass ID: %d\n", arr[i]->classID);
            currentClass = arr[i]->classID;
        } // end if()
        printf("\n\n******************************************************\nID: %d\t", arr[i]->ID);
        printf("Name: %s\t", arr[i]->name);
        printf("City: %s\t", arr[i]->city);
        printf("Class ID: %d\t", arr[i]->classID);
        printf("Date: %d/%d/%d\t\n", arr[i]->date.day, arr[i]->date.month, arr[i]->date.year);
    } // end for()
} // end printStdsByClass()

/*
    Function Name: findMinId()
    Input: tree root
    Output: std
    Function Operation: This function finds the student with the minimum ID in a tree.

*/
std findMinId(tree root)
{
    if (root == NULL)
    {
        return NULL;
    } // end if()
    else if (root->Left == NULL)
    {
        return root;
    } // end else if()
    else
    {
        return findMinId(root->Left);
    } // end else()
} // end findMinId()

/*
    Function Name: deleteStd()
    Input: int id, tree *root
    Output: void
    Function Operation: This function deletes a student from the tree.
*/
int deleteStd(int id, tree *root)
{
    if (*root == NULL)
    {
        return;
    } // end if()
    else if (id < (*root)->ID)
    {
        deleteStd(id, &(*root)->Left);
    } // end else if()
    else if (id > (*root)->ID)
    {
        deleteStd(id, &(*root)->Right);
    } // end else if()
    else
    {
        if ((*root)->Left == NULL && (*root)->Right == NULL)
        {
            free(*root);
            *root = NULL;
        } // end if()
        else if ((*root)->Left == NULL)
        {
            tree temp = *root;
            *root = (*root)->Right;
            free(temp);
        } // end else if()
        else if ((*root)->Right == NULL)
        {
            tree temp = *root;
            *root = (*root)->Left;
            free(temp);
        } // end else if()
        else
        {
            tree temp = findMinId((*root)->Right);
            (*root)->ID = temp->ID;
            strcpy((*root)->name, temp->name);
            strcpy((*root)->city, temp->city);
            (*root)->classID = temp->classID;
            (*root)->date = temp->date;
            deleteStd(temp->ID, &(*root)->Right);
        } // end else()
        return 1;
    } // end else()
} // end deleteStd()

/*
    Function Name: printMenue()
    Input: void
    Output: void
    Function Operation: This function prints the menue.
*/
void printMenue(void)
{
    printf("\nEnter Your Choice: ");
    printf("\n1. Add Student.");
    printf("\n2. Find Student & Update Student Info.");
    printf("\n3. List All Students.");
    printf("\n4. List All Students in a City.");
    printf("\n5. List All Students in each Class.");
    printf("\n6. Delete Student.");
    printf("\n7. Save Students in a File.");
    printf("\n8. Exit.");
} // end printMenue()

/*
    Function Name: saveTreeOnFile()
    Input: tree root
    Output: void
    Function Operation: This function saves the tree on a file.
*/
void saveTreeOnFile(tree root)
{
    FILE *out = fopen("students.data", "a");
    if (root != NULL)
    {
        saveTreeOnFile(root->Left);
        fprintf(out, "ID: %d || ", root->ID);   
        fprintf(out, "Name: %s || ", root->name);
        fprintf(out, "City: %s || ", root->city);
        fprintf(out, "Class ID: %d || ", root->classID);
        fprintf(out, "Date of Enrollment: %d/%d/%d\n", root->date.day, root->date.month, root->date.year);
        saveTreeOnFile(root->Right);
    } // end if()
    fclose(out);
} // end saveTreeOnFile()

/*
    Function Name: fileToTree()
    Input: void
    Output: tree
    Function Operation: This function reads a file and saves the data in a tree.
*/
tree fileToTree(void)
{
    FILE *in = fopen("students.data", "r");
    if (in == NULL)
    {
        printf("There is No \"students.data\" File!...\n\n");
        return NULL;
    } // end if()
    tree root = NULL;
    int id;
    char name[20];
    char city[10];
    int classID;
    Date date;
    while (fscanf(in, "ID: %d || ", &id) == 1)
    {

        fscanf(in, "Name: %s || ", name);
        fscanf(in, "City: %s || ", city);
        fscanf(in, "Class ID: %d || ", &classID);
        fscanf(in, "Date of Enrollment: %d/%d/%d\n", &date.day, &date.month, &date.year);
        insertStd(id, name, city, classID, date, &root);
    } // end while()
    return root;
} // end fileToTree()
