

/*//****************************************************************************************

Project : typing practice system Using C language

Made By :
    
    - Name : Atharva Vijay Khade 
    
    - Roll No: 19CSE1007
    
*///*****************************************************************************************



//_________________________________________________________________________________________________________________________________________________



#include<stdio.h>
#include<windows.h>                             
#include<stdbool.h>
#include<string.h>
#include<time.h>
#include<math.h>



//__________________________________________________________Definitions Section Start______________________________________________________________



#define max 1000000                                 // maximum length of the name and password

typedef struct details {

    char name[max];
    char password[max];
    int id;
    
} details; // strucutre details to store username and password

details new_entry;

void gotoxy(int x, int y)             // to handle the coordinates of the cursor
{
    COORD crd;
    crd.X = x;
    crd.Y = y;
    SetConsoleCursorPosition(GetStdHandle(STD_OUTPUT_HANDLE), crd);
}

void delay()                        // creating delay of 1000000 iterations 
{
    int delay = 0;
    while (delay < 1000000)
        delay++;
}



//__________________________________________________________Definitions Section End____________________________________________________________________






// _____________________________________________________Login / SignUp Form Section Start______________________________________________________________



void start_typing_practice_program(); 

void Create_Border_Login_SignUp_Page()   // creating display border of login and SignUp page, creates a rectangular running border
{

    gotoxy(55, 7);

    for (int i = 0; i < 100; i++)
    {
        printf("_");
        delay();
    }

    for (int i = 0; i < 30; i++)
    {
        gotoxy(155, 8 + i);
        printf("|");
        delay();
    }

    for (int i = 0; i < 100; i++)
    {
        gotoxy(155 - i - 1, 37);
        printf("_");
        delay();
    }

    for (int i = 0; i < 30; i++)
    {
        gotoxy(54, 37 - i);
        printf("|");
        delay();
    }

    return;

}

void Create_Containts()          // creates the containts of the login/SignUp page
{

    gotoxy(97, 10);

    delay();

    printf("Login / SignUp");

    delay();

    gotoxy(58, 16);
    printf("1]  Press  1  and  Enter  to  SignUp");

    delay();

    gotoxy(58, 23);
    printf("2]  Press  2  and  Enter  to  Login");

    delay();

    gotoxy(58, 31);
    printf("3]  Press  3  and  Enter  to  Exit");

    gotoxy(102, 35);

    return;

}

int login_SignUp_Page_Display()  // creating option menue for user to login or sign up or exit
{

    int option;
    int return_value = -1;

    Create_Border_Login_SignUp_Page();       // creates the border of the login page

    Create_Containts();                     // creates the contents of the page

    while (1)                         // creating the option selection functionality
    {

        scanf("%d", &option);

        if (option == '\n')
        {
            gotoxy(102, 35);
            continue;
        }

        switch (option)
        {
        case 1:
            return_value = 1;
            break;

        case 2:
            return_value = 2;
            break;

        case 3:
            return_value = 3;
            break;

        default:
            gotoxy(102, 35);
        }

        if (return_value == 1 || return_value == 2 || return_value == 3)
            break;

    }

    return return_value;

}


void SignUp_Login_Page_Display_MainPage()             // takes the username and password form the user and initializes the username and password
{

    char c;
    int i = 0;

    Create_Border_Login_SignUp_Page();       // creating the border

    gotoxy(70, 15);

    printf("Username : ");

    gotoxy(70, 20);

    printf("password : ");

    gotoxy(82, 15);

    fflush(stdin);

    while (1)                      // taking the username;
    {
        c = getchar();
        if (c == '\n')             // not taking '\n' in the username
            break;
        new_entry.name[i] = c;
        i++;
    }

    fflush(stdin);

    gotoxy(82, 20);

    i = 0;

    while (1)
    {
        c = getchar();
        if (c == '\n')
            break;
        new_entry.password[i] = c;
        i++;
    }

    return;

}


bool Display_TakeData_SignUp()       // to take the data and store it in the file and detect if username already present
{

    char put_new_line[] = "\n";
    bool already_exists = false;
    char checkName[max];
    int check = 1;
    char c;
    int i = 0;
    int j;

    SignUp_Login_Page_Display_MainPage();

    fflush(stdin);

    FILE *user_data;

    user_data = fopen("user_data.txt", "a+");

    while (!feof(user_data))
    {

        c = fgetc(user_data);

        if (check == 1)            // check username if already exists or not
        {

            if (c != '\n')
                checkName[i++] = c;
            else
            {
                for (j = 0; j < i; j++)
                    if (new_entry.name[j] != checkName[j])
                        break;
                if (j == i)
                    already_exists = true;
                if (already_exists)
                {

                    gotoxy(65, 28);
                    printf("UserName already Exists\n");
                    break;

                }
                j = 0;
                i = 0;
                check = 2;
            }

        }
        else if (c == '\n')
            check = 1;

    }

    if (already_exists == false)
    {

        fputs(new_entry.name, user_data);
        fputs(put_new_line, user_data);
        fputs(new_entry.password, user_data);
        fputs(put_new_line, user_data);
        fclose(user_data);

        FILE *id = fopen("id.txt", "a+");
        FILE *USER_ID = fopen("USER_ID.txt", "a+");
        char plus[] = "+";
        fputs(plus, USER_ID);
        fputs(new_entry.name, USER_ID);
        fputs(plus, USER_ID);
        int idi;
        char ch[1000];
        char c;
        int i = 0;
        while (!feof(id)) {
            c = fgetc(id);
            if (c == EOF)
                break;
            ch[i++] = c;
        }
        idi = atoi(ch);
        putw(idi, USER_ID);
        fputs(put_new_line, USER_ID);
        fclose(USER_ID);
        fclose(id);
        remove("id.txt");
        FILE *OG_id = fopen("id.txt", "a+");
        int number = idi + 1;
        int len = log10(number) + 1;
        char s[len];
        for (int i = len - 1; i >= 0; i--)
        {
            s[i] = ((number % 10) + 48);
            number /= 10;
            if (number == 0)
                break;
        }
        fputs(s, OG_id);
        fclose(OG_id);
    }
    else
    {

        fclose(user_data);
        gotoxy(65, 30);
        printf("Redirecting to sign up again\n");
        for (int i = 0; i < max; i++) {
            new_entry.name[i] = '\0';
            new_entry.password[i] = '\0';
        }
        return false;

    }

    gotoxy(65, 30);

    printf("Entering The World Of Typing, Tighten Your Seat Belts!!!");

    gotoxy(65, 33);

    for (i = 0; i < 70; i++)
    {
        printf("_");
        for (j = 0; j < 10; j++)
            delay();
    }

    return true;

}


bool Display_TakeData_Login()         // to take the data and verify the user if present or not with username and password
{
    char put_new_line[] = "\n";
    bool already_exists = false;
    char checkName[max];
    bool isPassword_verified = true;
    int check = 1;
    char c;
    int i = 0;
    int j;

    SignUp_Login_Page_Display_MainPage();

    fflush(stdin);

    FILE *user_data;

    user_data = fopen("user_data.txt", "a+");

    while (!feof(user_data))
    {

        c = fgetc(user_data);

        if (check == 1)            // check username if already exists or not
        {

            if (c != '\n')
                checkName[i++] = c;
            else
            {
                for (j = 0; j < i; j++)
                    if (new_entry.name[j] != checkName[j])
                        break;
                if (j == i)
                    already_exists = true;
                if (already_exists)
                {

                    // then we check password of that very entry

                    i = 0;
                    while (1)
                    {

                        c = fgetc(user_data);
                        if (c == '\n')
                            break;
                        else if (c != new_entry.password[i])
                        {
                            isPassword_verified = false;
                            break;
                        }
                        i++;
                    }

                    break;

                }
                j = 0;
                i = 0;
                check = 2;
            }

        }
        else if (c == '\n')
            check = 1;

    }

    if (already_exists == false)
    {

        fclose(user_data);
        gotoxy(65, 26);
        printf("Username Not Found\n");
        gotoxy(65, 30);
        printf("Redirecting to sign up first\n");
        for (int i = 0; i < max; i++) {
            new_entry.name[i] = '\0';
            new_entry.password[i] = '\0';
        }
        return false;

    }
    else
    {

        if (isPassword_verified == false)
        {

            fclose(user_data);
            gotoxy(65, 26);
            printf("Username Found\n");
            gotoxy(65, 30);
            printf("Please Recheck The Password, Restart The Application To Re-enter The Details\n");
            for (int i = 0; i < max; i++) {
                new_entry.name[i] = '\0';
                new_entry.password[i] = '\0';
            }
            return false;

        }

        fclose(user_data);

        gotoxy(65, 30);

        printf("Entering The World Of Typing, Tighten Your Seat Belts!!!");

        gotoxy(65, 33);

        for (i = 0; i < 70; i++)
        {
            printf("_");
            for (j = 0; j < 10; j++)
                delay();
        }

    }

    return true;

}


int Start_Select_Action()              // action of login or signup or exit selected here
{

    int option;                // to store the option selected from the login form

    option = login_SignUp_Page_Display();    // displays the login/ sign up page

    int call;
    switch (option)
    {

    case 1:                   // give the SignUp option
        system("cls");
        call = Display_TakeData_SignUp();
        if (call == 1)
            return 1;
        else
            return 0;
        getchar();
        break;

    case 2:                   // give the login option option
        system("cls");
        call = Display_TakeData_Login();
        if (call == 1)
            return 1;
        else
            return 0;
        getchar();
        break;

    case 3:                  // exit the program return to main function
        return -1;
        break;

    }

    return -1;

}



// ________________________________________________________Login / SignUp Form Section End_______________________________________________________





//________________________________________________________Main Menue Section Start_______________________________________________________________



void Create_Box_Main_Menue()               // Creates Main Menue Box
{

    int i = 0;
    int j = 0;

    gotoxy(5, 5);

    for (i = 0; i < 200; i++)
    {
        printf("_");
        delay();
    }

    for (i = 0; i < 40; i++)
    {
        gotoxy(205, 6 + i);
        delay();
        printf("|");
    }

    for (i = 0; i < 201; i++)
    {
        gotoxy(205 - i - 1, 45);
        printf("_");
        delay();
    }

    for (i = 0; i < 40; i++)
    {
        gotoxy(4, 45 - i);
        printf("|");
        delay();
    }

    return;

}

void Create_Menue()                       // Creates Menue Options
{

    int i = 0;
    int j = 0;

    gotoxy(65, 7);

    char welcome[] = "Welcome To The Typing Practice System  ---  Logged in as - ";

    char print_stm[max];

    for (i = 0; welcome[i] != '\0'; i++)
        print_stm[i] = welcome[i];

    for (; new_entry.name[j] != '\0'; i++, j++)
        print_stm[i] = new_entry.name[j];

    for (i = 0; print_stm[i] != '\0'; i++)
    {
        for (j = 0; j < 50; j++)
            delay();
        printf("%c", print_stm[i]);
    }

    gotoxy(10, 15);

    printf("1]  Learn  Touch  Typing  Lessons , Press  1  to  Learn");

    gotoxy(10, 20);

    printf("2]  Start  Typing  Practice , Press  2  to  start");

    gotoxy(10, 25);

    printf("3]  View  Your  Profile , Press  3  to  View");

    gotoxy(10, 30);

    printf("4]  View  Rank  List , Press  4  to  View");

    gotoxy(10, 35);

    printf("5]  Press  5  to  Exit");

}

void Learn_Touch_Typing_Page()            // Lessons for learning touch typing
{

    system("cls");

    printf("1.....\n");
    printf("\n");
    printf("What Is Touch Typing?\n\nTouch typing has nothing to do with a touch screen.\nBasically, touch typing is the ability to type with all your fingers without the need to look at the letters on the keyboard.\nYou achieve this not only by memorizing the placement of each letter, number, and sign on the keyboard, but by also memorizing which finger controls which keys.");

    printf("\n\n");
    printf("2.....\n");
    printf("\n");
    printf("The Benefits of Touch Typing\n\nIn the beginning it might look as if by using touch typing you will never\nbe able to type as fast and accurately as you do with your present two-finger system (also called hunt and peck), but this is not so.\nThe time you waste to move your two fingers cuts your speed drastically and even if you are an ultra-fast hunt and peck,\nthe number of words per minute you will be able to type is two, even three times lower than with touch typing.");

    printf("\n\n");
    printf("3.....\n");
    printf("\n");
    printf("How to Get Started with Touch Typing\n\nIt is also important to remember the home position of your fingers-Given as follows\n\n-Left pinky finger- On button A\n-Left ring finger- On button S\n-Left middle finger- On button D\n-Left index finger- On button F\n-Right index finger- On button J\n-Right middle finger- On button K\n-Right ring finger- On button L\n-Right pinky finger- On button ;\n-Both thumbs- On the Spacebar");

    printf("\n\n");
    printf("4.....\n");
    printf("\n");
    printf("You might have noticed that the letters F and J are marked (on any keyboard, not on learning ones only).\nThe idea is that thanks to these little markers you will feel the keys, and if you accidentally move your fingers from the correct position,\nyou will be able to place them back without having to look at the keyboard.");

    printf("\n\n");
    printf("5.....\n");
    printf("\n");
    printf("During typing, here are the movements of the fingers:\n\n-Left pinky finger- for typing button Q, A, Z and left Shift\n-Left ring finger- for typing button W, S and X\n-Left middle finger- for typing button E, D and C\n-Left index finger- for typing button R, F, V, T, G and B\n-Right index finger- for typing button Y, H, N, U, J and M\n-Right middle finger- for typing button I, K and ,\n-Right ring finger- for typing button O, L and .\n-Right pinky finger- for typing button P, ;, ?, {, }, ', Enter and right Shift\n-Both thumbs- for pressing the Spacebar\n\nYou will notice that you will need to stretch your index fingers (both left and right) to cover the buttons T and Y.\n\nIn a nutshell, this is the essence of touch typing. The rest is practice, and lots of it to get your fingers to memorize the movements");
    printf("\n\n");

    printf("Happy Typing   : )\n\n");

    printf("Enter any key and Enter to return to Main Menue\n");

    fflush(stdin);

    getchar();

    return;

}

void Reload_start_typing_practice_program()   // Reloading main menue after each chosen option
{

    system("cls");

    Create_Box_Main_Menue();

    Create_Menue();

    return;

}

void Load_Profile()
{
    system("cls");
    Create_Box_Main_Menue();
    getchar();
    gotoxy(7, 7);
    fflush(stdout);
    fflush(stdin);
    printf("User Name : %s", new_entry.name);

    // // processing the user speed data

    int speed_sum = 0;
    int speed_count = 0;
    int speeds[100];
    int ar_index = 0;
    char c;
    bool isempty = true;
    for (int i = 0; i < 100; i++)
        speeds[i] = -1;

    FILE *speed_records = fopen("speed_records.txt", "a+");

    while (!feof(speed_records)) {
        c = fgetc(speed_records);
        if (c == '+') // search for user
        {
            bool found = true;
            int index = 0;
            while (1)
            {
                c = fgetc(speed_records);
                if (c == '-')
                    break;
                if (c != new_entry.name[index++])
                {
                    found = false;
                    break;
                }
            }
            if (found)
            {
                int speed = getw(speed_records);
                speed_count++;
                speed_sum += speed;
                speeds[ar_index++] = speed;
                isempty = false;
            }
        }
    }

    if (isempty == true)
    {
        fflush(stdin);
        getchar();
        return;
    }

    int speed = speed_sum / speed_count;
    gotoxy(7, 9);
    fflush(stdout);
    fflush(stdin);
    printf("User Speed : %d WPM", speed);

    gotoxy(7, 11);
    fflush(stdout);
    fflush(stdin);
    printf("The User Typing Record is as follows : ");

    gotoxy(7, 13);
    for (int i = 0; speeds[i] != -1; i++)
    {
        fflush(stdout);
        fflush(stdin);
        printf("%d] - %d WPM", i + 1, speeds[i]);
        gotoxy(7, 13 + i + 1);
    }
    fflush(stdout);
    fflush(stdin);
    gotoxy(10, 36);
    printf("Keep Practicing : ) Enter any key to return to main menue");
    fflush(stdout);
    fflush(stdin);
    fclose(speed_records);
    getchar();
    fflush(stdout);
    fflush(stdin);
}

void Practice()                               // Implementation of practice section
{

    system("cls");                           // clearing previous screen
    FILE *lines;                            // to retrive the lines from the file
    int count_lines = 0;
    char c;
    int chose_random_line;
    char random_line[200];
    int i = 1;
    int j = 0;
    int k = 0;
    long double start_time;
    long double end_time;
    long double total_time;
    float errors = 0;
    float correct = 0;
    float length_to_type;
    float accuracy;
    float wpm;
    float words;
    int line_index;

    Create_Box_Main_Menue();                    // creating box aroound display

    for (i = 0; i < 200; i++)
        random_line[i] = '\0';

    i = 1;

    gotoxy(7, 7);

    printf("INSTRUCTIONS FOR PRACTICE");

    gotoxy(7, 7 + 2);

    printf("You will be given a random string on the screen and you have to type that as it is");

    gotoxy(7, 8 + 2);

    printf("Where ever the there is blank it is to be typed as <space> {ie: no tabs are there}");

    gotoxy(7, 9 + 2);

    printf("Your total time of typing will be noted and the speed will be calculated accordingly");

    gotoxy(7, 10 + 2);

    gotoxy(7, 11 + 2);

    printf("The overall speed in words per minute <wpm> and the accuracy will be displayed once you have finished typing");

    gotoxy(7, 12 + 2);

    printf("Make sure to hit the Enter once you have finished typing the given line, Press any key and then enter to begin --- All The Best");

    gotoxy(7, 16);

    for (i = 0; i < 195; i++)
    {
        printf("_");
        delay();
    }

    fflush(stdin);

    getchar();

    gotoxy(7, 18);

    printf("Type The Given Line");

    fflush(stdin);

    gotoxy(7, 20);

    lines = fopen("lines.txt", "a+");            // opening file to count number of lines

    while (!feof(lines))                       // counting total lines in the file having lines to type
    {
        c = fgetc(lines);
        if (c == '\n')
            count_lines++;
    }

    fclose(lines);

    chose_random_line = ((abs((int)time(NULL))) % count_lines);     // picking random line from the file

    chose_random_line++;


    FILE *lines2 = fopen("lines_2.txt", "a+");

    i = 1;
    j = 0;
    line_index = 0;

    fflush(stdin);

    while (1)                           // picking the random line as taken from the random function
    {
        c = fgetc(lines2);
        if (c == '\n')
            line_index++;
        if (line_index == chose_random_line)
        {
            c = fgetc(lines2);
            while (c != '\n') {
                random_line[j++] = c;
                c = fgetc(lines2);
            }
            break;
        }
    }

    fclose(lines2);

    printf("%s", random_line);                    // line to print

    gotoxy(7, 22);

    for (int i = 0; i < 195; i++)
    {
        printf("_");
    }

    gotoxy(7, 24);

    printf("Start in- ");
    for (int i = 0; i < 500; i++)
        delay();
    printf("3 ");
    for (int i = 0; i < 500; i++)
        delay();
    printf("2 ");
    for (int i = 0; i < 500; i++)
        delay();
    printf("1 ");
    for (int i = 0; i < 500; i++)
        delay();
    printf("Start Now");

    gotoxy(7, 27);

    time_t seconds;
    seconds = time(NULL);                         // measuring the start time of typing in seconds
    time_t end;

    i = 0;
    words = 0;
    fflush(stdin);

    // Using string to check for errors

    int check[500];
    for (int i = 0; i < 500; i++)
        check[i] = 0;
    length_to_type = 0;
    for (int i = 0; random_line[i] != '\0'; i++)
    {
        check[random_line[i]]++;
        length_to_type++;
    }

    while (1)
    {
        c = getchar();
        if (c == '\n')
        {
            end = time(NULL);              // measuring end time of typing
            break;
        }
        if (c == ' ')
            words++;
        i++;
        check[c]--;
    }

    fflush(stdin);
    words++;

    for (int i = 0; i < 500; i++)          // counting the errors
        if (check[i] < 0 || check[i] > 0)
            errors = errors + abs(check[i]);

    total_time = (((double)(end - seconds)) / 60);        // total time in minutes

    accuracy = 100 - (errors / length_to_type) * 100;           // calculating accuracy

    wpm = words / total_time;                                   // calculating wpm speed

    gotoxy(69, 33);

    printf("Score Card : )");

    gotoxy(69, 36);

    printf("Speed = %f WPM", wpm);                               // displaying the result

    gotoxy(69, 40);

    printf("Accuracy = %f percentage", accuracy);

    getchar();

    gotoxy(69, 43);

    // Update the user's speed table
    FILE *speed_records = fopen("speed_records.txt", "a+");

    char p[] = "+";
    char m[] = "-";
    char n[] = "\n";
    fputs(p, speed_records);
    fputs(new_entry.name, speed_records);
    fputs(m, speed_records);
    int num = wpm;
    putw(num, speed_records);
    fputs(n, speed_records);
    fclose(speed_records);

    printf("Score Updated Successfully, Press any key to return to main menue");

    int choice;
    scanf("d", &choice);

    return;

}

void Load_Rank_List()
{

    system("cls");
    Create_Box_Main_Menue();
    getchar();
    gotoxy(100, 7);
    printf("Rank List");
    // getchar();

    typedef struct user_and_speed
    {
        char username[100];
        int speed;
        int cnt;
        
    } user_and_speed;

    char nul[] = "";

    user_and_speed data[1000];

    // default values
    for (int i = 0; i < 1000; i++) {
        strcpy(data[i].username, nul);
        data[i].speed = -1;
        data[i].cnt=0;
    }

    // now we place data[user_id] as the username and it's speed and then sort that array according to the speed

    FILE *user_id = fopen("USER_ID.txt", "a+");
    char c;

    while (!feof(user_id))
    {
        c = fgetc(user_id);
        if (c == '+')
        {
            char temp[100];
            for (int i = 0; i < 100; i++)
                temp[i] = '\0';
            c = fgetc(user_id);
            int index = 0;
            while (c != '+')
            {
                temp[index++] = c;
                c = fgetc(user_id);
            }
            int hash_index = getw(user_id);
            strcpy(data[hash_index].username, temp);
            // getting the user's speed from the speed speed_records
            FILE *sr = fopen("speed_records.txt", "a+");
            char q;
            while (!feof(sr))
            {
                q = fgetc(sr);
                if (q == '+')
                {
                    bool isuser = true;
                    int iterator = 0;
                    q = fgetc(sr);
                    while (q != '-')
                    {
                        if (q != temp[iterator++]) {
                            isuser = false;
                            break;
                        }
                        q = fgetc(sr);
                    }
                    if (isuser)
                    {
                        data[hash_index].speed += getw(sr);
                        data[hash_index].cnt++;
                    }
                }
            }
            fclose(sr);
        }
    }
    
    for(int i=0;i<100;i++){
        if(data[i].speed!=-1){
            data[i].speed+=1;
            data[i].speed/=data[i].cnt;
        }
    }

    fclose(user_id);
    int print_index = 1;
    int row = 0;

    for (int j = 0; j < 100; j++) {

        int spd = -1;
        int max_ind = -1;

        for (int i = 0; i < 100; i++)
        {
            if (data[i].speed > spd)
            {
                spd = data[i].speed;
                max_ind = i;
            }
        }
        if (spd != -1) {
            gotoxy(10, 10 + row);
            row++;
            printf("%d] ", print_index);
            print_index++;
            printf("%s", data[max_ind].username);
            printf(" - %d WPM", data[max_ind].speed);
            data[max_ind].speed = -1;
        }
        else
            break;

    }

    getchar();
    return;

}

void start_typing_practice_program()
{

    system("cls");

    Reload_start_typing_practice_program();

    int option;

    while (1)
    {

        gotoxy(30, 40);
        scanf("%d", &option);

        if (option == 5)                // exit program;
            return;
        else if (option == 1)
        {
            Learn_Touch_Typing_Page();
            Reload_start_typing_practice_program();
            fflush(stdin);
        }
        else if (option == 2)
        {
            Practice();
            Reload_start_typing_practice_program();
            fflush(stdin);
        }
        else if (option == 3)
        {
            Load_Profile();
            Reload_start_typing_practice_program();
            fflush(stdin);
        }
        else if (option == 4)
        {
            Load_Rank_List();
            Reload_start_typing_practice_program();
            fflush(stdin);
        }
        else
            continue;

    }


    getchar();

}


//_____________________________________________________Main Menue Section End________________________________________________________________________________


int main()
{



    printf("Press Any To Start The Application, Make Sure Your Terminal / Console Window Is In Maximized Mode\n");

    getchar();

    system("cls");

    int Login_SignUp_Successful;

    while (1) {

        system("cls");
        Login_SignUp_Successful = Start_Select_Action();

        if (Login_SignUp_Successful == 0) {
            getchar();
            continue;
        }
        else if (Login_SignUp_Successful == 1)
        {
            // start the program
            start_typing_practice_program();
            break;
        }
        else if (Login_SignUp_Successful == -1)
        {
            break;
        }
    }

    return 0;
    
    

}