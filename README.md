# tinashe
BMIcalculator
import java.util.Scanner;
import java.util.Locale;

public class basics {

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        sc.useLocale(Locale.US);
        char repeat;

        System.out.println("What would you like to calculate: \n1." + 
		                   " BMI -> Body Mass Index \n2. IBW -> Ideal Body Weight" +
						   "\n3. BFP -> Body Fat Percentage" );
        int calc_Type = sc.nextInt();

        switch (calc_Type) {
            case 1: // BMI Calculation
                do {
                    int unitChoice = getUnitChoice(sc);

                    double weight = (unitChoice == 1) ? getValidInput(sc, "Enter weight in kgs: ", 10, 600)
                            : getValidInput(sc, "Enter weight in pounds: ", 22, 1300); //ternary operation to check a person

                    double height = (unitChoice == 1) ? getValidInput(sc, "Enter height in metres: ", 0.5, 2.5)
                            : getValidInput(sc, "Enter your in inches: ", 20, 100);

                    double bmi = calculateBMI(unitChoice, weight, height);
                    System.out.printf("BMI: %.2f%n", bmi);
                    repeat = askToRepeat(sc);
                    System.out.println();

                } while (repeat == 'Y' || repeat == 'y');
                break;

            case 2: // IBW Calculation
                do {
                    int unitChoice2 = getUnitChoice2(sc);

                    double height2 = (unitChoice2 == 1) ? getValidInput(sc, "Enter your height in inches: ", 60, 100)
                            : getValidInput(sc, "Enter your height in inches: ", 60, 100);

                    double idw = calculateIDW(unitChoice2, height2);
                    System.out.printf("IDW: %.2f kg%n", idw);
                    repeat = askToRepeat(sc);
                    System.out.println();

                } while (repeat == 'Y' || repeat == 'y');
                break;
			case 3: //Body fat percentage
			    System.out.println("Enter gender, select 1(male) or 0(female)): ");
				int gender = sc.nextInt();
				System.out.println("Enter estimated Body index mass(MBI): ");
				int mbi2 = sc.nextInt();
				System.out.println("Age: ");
				int age = sc.nextInt();
				
				//Body fat percentage using MBI-based estimate
				double bfp =  (double) (1.20 * mbi2) + (0.23* age) - (10.8 * gender) - 5.4;
				
				System.out.println("Estimated body fat is: " + bfp);
			    break;

            default:
                System.out.println("Invalid choice.");
                break;
        }

        sc.close(); // Close the scanner
    }

    public static char askToRepeat(Scanner sc) {
        System.out.print("Do you want to calculate again? (Y/N): ");
        return sc.next().charAt(0);
    }

    public static int getUnitChoice(Scanner sc) {
        int choice;

        while (true) {
            System.out.println("Select a preferred unit:\n" + "1. Metric (kg, m) \n" + "2. Imperial (lbs, in) \n"
                    + "Select one of the options above.");
            if (sc.hasNextInt()) {
                choice = sc.nextInt();
                if (choice == 1 || choice == 2) {
                    break;
                } else {
                    System.out.print("Invalid choice. Enter either 1 or 2: ");
                }
            } else {
                System.out.print("Invalid input. Enter either 1 or 2: ");
                sc.next();
            }
        }

        return choice;
    }

    public static int getUnitChoice2(Scanner sc) {
        int choice2;

        while (true) {
            System.out.println("Select a preferred gender:\n" + "1. Men \n" + "2. Women \n"
                    + "Select one of the options above.");
            if (sc.hasNextInt()) {
                choice2 = sc.nextInt();
                if (choice2 == 1 || choice2 == 2) {
                    break;
                } else {
                    System.out.print("Invalid choice. Enter either 1 or 2: ");
                }
            } else {
                System.out.print("Invalid input. Enter either 1 or 2: ");
                sc.next();
            }
        }

        return choice2;
    }

    public static double getValidInput(Scanner sc, String prompt, double min, double max) {
        double value;

        while (true) {
            System.out.println(prompt);

            if (sc.hasNextDouble()) {
                value = sc.nextDouble();
                if (value >= min && value <= max) {
                    break;
                } else {
                    System.out.printf("Enter a value between %.1f and %.1f \n", min, max);
                }
            } else {
                System.out.print("Invalid input. Enter a value: ");
                sc.next();
            }
        }
        return value;
    }

    public static double calculateIDW(int unitChoice, double ht2) {
        double totalIDW;

        switch (unitChoice) {
            case 1: // For Men
                totalIDW = 50.0 + 2.3 * (ht2 - 60);
                break;
            case 2: // For Women
                totalIDW = 45.5 + 2.3 * (ht2 - 60); // Example formula for women
                break;
            default:
                totalIDW = 0; // Default case
                break;
        }

        return totalIDW;
    }

    public static double calculateBMI(int unitChoice, double wt, double ht) {
        double totalBMI;

        if (unitChoice == 1) {
            totalBMI = wt / (ht * ht);
        } else{
            totalBMI = (703 * wt) / (ht * ht); // Corrected formula
        }

        return totalBMI;
    }
