package School;

import java.util.Scanner;

class information_student {

    String name, birth, addrees, rating;
    int phone, sum;
    int degrees[] = new int[8];
    double average;
}

public class School{

    static int NumperStudent = 20;
    static Scanner in = new Scanner(System.in);
    static information_student[][] personal = new information_student[12][NumperStudent];
    static information_student[][] temp = new information_student[12][NumperStudent];
    static information_student[][] successful = new information_student[12][NumperStudent];
    static information_student[][] fail = new information_student[12][NumperStudent];

    public static void main(String[] args) {
        int[] FailCounter = new int[12];
        int[] SuccessfulCounter = new int[12];
        int[] key = {1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1};
        int[] key2 = {1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1};
        int[] StudentCounter = {0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0};
        int[] DegreesCounter = {0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0};
        int Class = 0, choose, closed = 1;
        int[] NumperSubjects = {5, 6, 6, 6, 6, 6, 7, 7, 7, 8, 8, 8};
        String names;
        String Classes[] = {"frist grade", "second grade", "third grade",
            "fourth grade", "fifh grade", "sixth grade",
            "Seventh intermediate grade", "Eighth intermediate grade", "Ninth intermediate grade",
            "frist grade secondary", "second grade  secondary", "third grade  secondary"};
        String subjects[] = {"The holy quran", "Islamic education", "Arabic language",
            "biology", "History", " English language", "Phyiscs", "Chemistry"};
        for (int i = 0;; i++) {
            System.out.println("1_To viow the classes\n"
                    + "2_viow top classes\n"
                    + "3_To go out...");
            int choosenumper = in.nextInt();
            if (choosenumper == 2) {
                print_top(key2, Classes, SuccessfulCounter, successful, NumperSubjects, subjects);
            } else if (choosenumper == 3) {
                System.out.println("thank you");
                break;
            } else if (choosenumper == 1) {
                for (int again = 0;; again++) {
                    System.out.println("Shoose numpur any class");
                    for (int j = 0; j < Classes.length; j++) {
                        System.out.println((j + 1) + "_ " + Classes[j]);
                    }
                    System.out.println("0_To go out, click numper");
                    Class = in.nextInt();
                    if (Class == 0) {
                        break;
                    }
                    Class = Class - 1;
                    closed = 1;
                    do {
                        if (Class > 12 || Class < 0) {
                            print_error();
                            break;
                        }
                        System.out.println("Welcome in clas " + Classes[Class]);
                        System.out.println("Choose numper\n"
                                + "1_To register students.\n"
                                + "2_To enter students' grades or to edit\n"
                                + "3_To print the student list\n"
                                + "4 To print the Student' degrres\n"
                                + "5 To search any student in class\n"
                                + "6 To go out, ");
                        choose = in.nextInt();
                        switch (choose) {
                            case 1: {
                                key[Class] = 0;
                                insert_information(Classes, Class, StudentCounter, personal);
                                break;
                            }
                            case 2: {
                                if (key[Class] == 1) {
                                    System.out.println("error .Dont have students");
                                    break;
                                } else {
                                    key2[Class] = 0;
                                    insert_degrees(DegreesCounter, Class, StudentCounter, personal, NumperSubjects, subjects, temp);
                                    attibution(StudentCounter, Class, temp, personal);
                                    arranged_totsl(StudentCounter, Class, temp);
                                    FailCounter[Class] = 0;
                                    SuccessfulCounter[Class] = 0;
                                    for (int j = 0; j < DegreesCounter[Class]; j++) {
                                        if (temp[Class][j].rating.equals("fail")) {
                                            fail[Class][FailCounter[Class]] = temp[Class][j];
                                            FailCounter[Class] += 1;
                                        } else {
                                            successful[Class][SuccessfulCounter[Class]] = temp[Class][j];
                                            SuccessfulCounter[Class] += 1;
                                        }
                                    }
                                }
                                break;
                            }
                            case 3: {
                                if (key[Class] == 1) {
                                    System.out.println("error .Dont have students");
                                    break;
                                }
                                print_information(StudentCounter, Class, personal);

                                break;
                            }
                            case 4: {
                                if (key[Class] == 1) {
                                    System.out.println("error .Dont have students");
                                    break;
                                }
                                if (key[Class] == 1 || key2[Class] == 1) {
                                    System.out.println("error .Dont have students");
                                    break;
                                }
                                System.out.println("choose numper\n 1 for print sum\n2 for print name\n"
                                        + "3 for print successfuls and faills");
                                choose = in.nextInt();
                                int begining_count = 0;
                                if (choose == 1) {
                                    attibution(StudentCounter, Class, temp, personal);
                                    arranged_totsl(StudentCounter, Class, temp);
                                    print(NumperSubjects, Class, subjects, StudentCounter, begining_count, temp);
                                } else if (choose == 2) {
                                    attibution(StudentCounter, Class, temp, personal);
                                    arranged_name(StudentCounter, Class, temp);
                                    print(NumperSubjects, Class, subjects, StudentCounter, begining_count, temp);
                                } else if (choose == 3) {
                                    attibution(StudentCounter, Class, temp, personal);
                                    arranged_totsl(StudentCounter, Class, temp);
                                    System.out.println("list of successful studends ");
                                    print(NumperSubjects, Class, subjects, SuccessfulCounter, begining_count, successful);
                                    if (FailCounter[Class] == 0) {
                                        System.out.println("dont have fail students in Class " + Classes[Class]);
                                    } else {
                                        System.out.println("list of successful studends ");
                                        print(NumperSubjects, Class, subjects, FailCounter, begining_count, fail);
                                    }
                                } else {
                                    print_error();
                                }
                                break;
                            }
                            case 5: {
                                if (key[Class] == 1) {
                                    System.out.println("error .Dont have students");
                                    break;
                                }
                                System.out.println("choose name ");
                                names = in.nextLine();
                                names = in.nextLine();
                                boolean search_test = false;
                                choose = 0;
                                for (int j = 0; j < StudentCounter[Class]; j++) {
                                    if (personal[Class][j].name.equalsIgnoreCase(names)) {
                                        System.out.println("Yes.There is a student whith this name ");
                                        System.out.println("enter numper\n 1_print the information  "
                                                + "\n2 for print the the degrees"
                                                + "\n3 for print degrees and informtion");
                                        choose = in.nextInt();
                                    }
                                    switch (choose) {
                                        case 1: {
                                            search_test = false;
                                            choose = 0;
                                            print_student(personal, Class, j);
                                            break;
                                        }
                                        case 2: {
                                            search_test = true;
                                            if (personal[Class][0].degrees[0] == 0) {
                                                System.out.println("There are not student grades yet.\n");
                                                choose = 0;
                                                break;
                                            }

                                            attibution(StudentCounter, Class, temp, personal);

                                            arranged_totsl(StudentCounter, Class, temp);
                                            int Temp = StudentCounter[Class];
                                            StudentCounter[Class] = i + 1;
                                            print(NumperSubjects, Class, subjects, StudentCounter, j, temp);
                                            StudentCounter[Class] = Temp;
                                            break;
                                        }
                                        case 3: {
                                            search_test = true;
                                            print_student(personal, Class, j);
                                            if (personal[Class][0].degrees[0] == 0) {
                                                System.out.println("There are not student grades yet.");
                                                choose = 0;
                                                break;
                                            }
                                            choose = 0;
                                            attibution(StudentCounter, Class, temp, personal);

                                            int Temp = StudentCounter[Class];
                                            StudentCounter[Class] = j + 1;
                                            print(NumperSubjects, Class, subjects, StudentCounter, j, temp);
                                            StudentCounter[Class] = Temp;
                                            break;
                                        }
                                        case 0: {
                                            break;
                                        }
                                        default: {
                                            print_error();
                                            choose = 0;
                                            search_test = true;
                                        }
                                    }
                                }
                                if (search_test == false) {
                                    System.out.println("There are not student");
                                }
                                break;
                            }
                            case 6: {
                                closed = 0;
                                break;
                            }
                            default: {
                                print_error();
                            }
                        }
                    } while (closed == 1);
                }
            } else {
                print_error();
            }
        }
    }

    static information_student[][] attibution(int[] StudentCounter, int Class, information_student[][] temp, information_student[][] personal) {
        for (int i = 0; i < StudentCounter[Class]; i++) {
            temp[Class][i] = personal[Class][i];
        }
        return temp;
    }

    static void print_top(int key2[], String Classes[], int[] SuccessfulCounter, information_student[][] successful, int[] NumperSubjects,
            String subjects[]) {
        System.out.println("How many students do you want in each class?");
        int NumperTop = in.nextInt();
        for (int Class = 0; Class < 12; Class++) {
            if (key2[Class] == 1) {
                System.out.println("Dont have students in class " + Classes[Class]);

            } else {
                streak();
                System.out.println("Top students in class " + Classes[Class]);
                arranged_totsl(SuccessfulCounter, Class, temp);
                int Temp;
                Temp = SuccessfulCounter[Class];
                if (SuccessfulCounter[Class] >= NumperTop) {
                    SuccessfulCounter[Class] = NumperTop;
                }
                System.out.println("There ar " + SuccessfulCounter[Class] + " students ");
                int begining_count = 0;
                if (SuccessfulCounter[Class] == 0) {
                    System.out.println("dont have fail students in Class ");
                } else {
                    print(NumperSubjects, Class, subjects, SuccessfulCounter, begining_count, successful);
                    streak();
                }

                SuccessfulCounter[Class] = Temp;
            }
        }
    }

    static void streak() {
        for (int i = 0; i < 5; i++) {
            System.out.print("_____________________________________________");
        }
        System.out.println("_____________________________________________");
    }

    static information_student[][] insert_degrees(int DegreesCounter[], int Class, int StudentCounter[], information_student[][] personal, int NumperSubjects[],
            String subjects[], information_student[][] temp) {
        System.out.println("enter the studrnt degrees\n" + "Choose \n");
        for (int i = DegreesCounter[Class]; i < StudentCounter[Class]; i++) {
            System.out.println((i + 1) + "_" + personal[Class][i].name);
        }
        for (int i = DegreesCounter[Class]; i < StudentCounter[Class]; i++) {
            ++DegreesCounter[Class];
            System.out.println("enter the studrnt degrees " + personal[Class][i].name);
            boolean app = false;
            for (int j = 0; j < NumperSubjects[Class]; j++) {

                System.out.print(subjects[j] + "= ");
                personal[Class][i].degrees[j] = in.nextInt();
                if (personal[Class][i].degrees[j] > 100 || personal[Class][i].degrees[j] < 0) {
                    --j;
                    System.out.println("sory.wrong entry.again");
                } else {
                    if (personal[Class][i].degrees[j] < 50) {
                        app = true;
                    }

                    personal[Class][i].sum += personal[Class][i].degrees[j];
                }
            }
            double divison = 1.0 * NumperSubjects[Class];
            personal[Class][i].average = temp[Class][i].sum / divison;
            temp[Class][i].rating = (app == true) ? "fail" : (personal[Class][i].average >= 85) ? "excellent" : (personal[Class][i].average >= 70) ? "good" : "acceptable";
            attibution(StudentCounter, Class, temp, personal);
        }
        return personal;
    }

    static void print_error() {
        System.out.println("wrong choice");
    }

    static information_student[][] print(int NumperSubjects[], int Class, String subjects[], int[] StudentCounter, int begining_count, information_student[][] temp) {
        System.out.print("NU  " + "NAME");
        for (int i = 0; i < NumperSubjects[Class]; i++) {
            System.out.print(" \t" + subjects[i] + "\t");
        }
        System.out.print("\tsum" + "\t average" + "\t\t    Appreciation");
        System.out.println();
        for (int i = begining_count; i < StudentCounter[Class]; i++) {
            System.out.print((i + 1) + "_  " + temp[Class][i].name + " :_\t");
            for (int j = 0; j < NumperSubjects[Class]; j++) {
                System.out.print(temp[Class][i].degrees[j] + "\t \t \t");
            }
            System.out.println(temp[Class][i].sum + "\t " + temp[Class][i].average + "\t\t" + temp[Class][i].rating);
        }
        return temp;
    }

    static information_student[][] arranged_totsl(int StudentCounter[], int Class, information_student[][] temp) {
        for (int i = 0; i < StudentCounter[Class]; i++) {
            for (int j = 0; j < StudentCounter[Class]; j++) {
                if (temp[Class][i].sum > temp[Class][j].sum) {
                    information_student Temp = temp[Class][j];
                    temp[Class][j] = temp[Class][i];
                    temp[Class][i] = Temp;
                }
            }
        }

        return temp;
    }

    static information_student[][] arranged_name(int[] StudentCounter, int Class, information_student[][] temp) {
        for (int i = 0; i < StudentCounter[Class] - 1; i++) {
            for (int j = 0; j < StudentCounter[Class] - i - 1; j++) {
                if (temp[Class][j].name.charAt(0) > temp[Class][j + 1].name.charAt(0)) {
                    information_student Temp = temp[Class][j];
                    temp[Class][j] = temp[Class][j + 1];
                    temp[Class][j + 1] = Temp;
                }
            }
        }

        return temp;
    }

    static void print_student(information_student[][] personal, int x, int i) {
        System.out.println("UN \t name \t address   date of birth\tyour phone");
        System.out.print(personal[x][i].name + "\t  " + personal[x][i].addrees + "\t  " + personal[x][i].birth + "\t" + personal[x][i].phone);
        System.out.print("\n");

    }

    static void print_information(int StudentCounter[], int Class, information_student[][] personal) {
        System.out.println("UN \t name \t address  \t date of birth \tyour phone");
        for (int i = 0; i < StudentCounter[Class]; i++) {
            System.out.print((i + 1) + "_\t" + personal[Class][i].name + "\t  " + personal[Class][i].addrees + "\t  " + personal[Class][i].birth + "\t" + personal[Class][i].phone);
            System.out.print("\n");
        }
    }

    static information_student[][] insert_information(String Classes[], int Class, int[] StudentCounter, information_student[][] personal) {
        System.out.println("Enter the information of the students for " + Classes[Class]);
        for (int i = StudentCounter[Class]; i < NumperStudent; i++) {
            personal[Class][i] = new information_student();
            ++StudentCounter[Class];
            System.out.println("enter the name of student.numper _" + (i + 1));
            personal[Class][i].name = in.nextLine();
            personal[Class][i].name = in.nextLine();
            System.out.println("enter the address of student " + personal[Class][i].name);
            personal[Class][i].addrees = in.nextLine();
            System.out.println("enter the student birth date " + personal[Class][i].name);
            personal[Class][i].birth = in.nextLine();
            System.out.println("enter the  student phone " + personal[Class][i].name);
            personal[Class][i].phone = in.nextInt();
            if (StudentCounter[Class] == NumperStudent) {
                System.out.println("The numper is complete!");
                break;
            }
            System.out.println("To continue, click on any numper."
                    + "\nTo go out, click numper one.");
            int continuation = in.nextInt();
            if (continuation == 1) {
                break;
            }

        }
        attibution(StudentCounter, Class, temp, personal);
        return personal;
    }
}  
