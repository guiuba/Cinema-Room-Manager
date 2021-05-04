# Cinema-Room-Manager
My solution to the Cinema-Room-Manager, a Jet Brains Academy project stage 5/5
It's an application that sell tickets, check available seats, see sales statistics, and more.

Works like the example below:

![image](https://user-images.githubusercontent.com/69851038/117050208-7c70ee80-aceb-11eb-934d-cc6342a73b15.png)

---------------------------------------------------------------------
My code:

import java.util.Scanner;

public class Cinema {
    static Scanner scan = new Scanner(System.in);
    static double soldTkts = 0;
    static double totalSeats;
    static int currentIncome = 0;

    public static void main(String[] args) {

        System.out.println("Enter the number of rows:");
        int rows = scan.nextInt() + 1;
        System.out.println("Enter the number of seats in each row:");
        int seats = scan.nextInt() + 1;
        totalSeats = (rows - 1) * (seats - 1);
        String[][] cinema = new String[rows][seats];
        cinema = cinemaSetUp(cinema, rows, seats);
        boolean isOver = false;
        while (!isOver) {
            switch (chooseAction()) {
                case 1:
                    printCinemaRoom(cinema);
                    break;
                case 2:
                    cinema = buyTkt(cinema, rows, seats);
                    break;
                case 3:
                    getStatistics(rows, seats);
                    break;
                case 0:
                    isOver = true;
                    break;
                default:
                    System.out.println("invalid entry, try again");
                    break;
            }
        }
        scan.close();
    }

    static void getStatistics(int rows, int seats) {
        System.out.println("Number of purchased tickets: " + (int) soldTkts);
        System.out.printf("Percentage: %.2f%%%n", (soldTkts / totalSeats) * 100);
        System.out.printf("Current income: $%d%n", currentIncome);
        System.out.printf("Total income: $%d%n", calculateTotalIncome(rows, seats));
    }

    static String[][] buyTkt(String[][] cinema, int rows, int seats) {
        boolean isValidInput = false;
        while (!isValidInput) {
            System.out.println("Enter a row number:");
            int auxRowNumber = scan.nextInt();
            System.out.println("Enter a seat number in that row:");
            int auxSeatNumber = scan.nextInt();

            if (auxRowNumber > rows - 1 || auxSeatNumber > seats - 1) {
                System.out.println("Wrong input!");

            } else if (cinema[auxRowNumber][auxSeatNumber].equals("B") ||
                    cinema[auxRowNumber][auxSeatNumber].equals(" ")) {
                System.out.println("That ticket has already been purchased!");
            } else {
                cinema[auxRowNumber][auxSeatNumber] = "B";
                soldTkts++;
                currentIncome += calculateTktPrice(rows, seats, auxRowNumber);
                System.out.println("\nTicket price: $" + calculateTktPrice(rows, seats, auxRowNumber));
                isValidInput = true;
            }
        }
        return cinema;
    }

    static int chooseAction() {
        System.out.println(
                "\n1. Show the seats\n" +
                        "2. Buy a ticket\n" +
                        "3. Statistics\n" +
                        "0. Exit");
        return scan.nextInt();
    }

    static String[][] cinemaSetUp(String[][] cinema, int rows, int seats) {
        for (int row = 0; row < rows; row++) {
            for (int seat = 0; seat < seats; seat++) {
                cinema[row][seat] = "S";
                cinema[0][0] = " ";
                if (row == 0 && seat > 0) {
                    cinema[0][seat] = String.valueOf(seat);
                }
                if (row > 0 && seat == 0) {
                    cinema[row][0] = String.valueOf(row);
                }
            }
        }
        return cinema;
    }

    static void printCinemaRoom(String[][] cinema) {
        System.out.println();
        System.out.println("Cinema:");
        for (String[] cinem : cinema) {
            for (String cine : cinem) {
                System.out.print(cine + " ");

            }
            System.out.println();
        }
        System.out.println();
    }

    static int calculateTotalIncome(int rows, int seats) {
        int totalIncome = 0;
        --rows;
        --seats;
        if (rows * seats <= 60) {
            totalIncome = rows * seats * 10;
        } else {
            if (rows % 2 == 0) {
                totalIncome += ((rows * seats) / 2) * 10;
                totalIncome += ((rows * seats) / 2) * 8;
            } else {
                totalIncome += seats * (rows / 2) * 10;
                totalIncome += seats * ((rows / 2) + 1) * 8;
            }
        }
        return totalIncome;
    }

    static int calculateTktPrice(int rows, int seats, int rowNumber) {
        int tktPrice;
        --rows;
        --seats;
        if (rows * seats <= 60) {
            tktPrice = 10;
        } else {
            if (rows % 2 == 0) {
                if (rowNumber < rows / 2) {
                    tktPrice = 10;
                } else {
                    tktPrice = 8;
                }

            } else {
                if (rowNumber <= rows / 2) {
                    tktPrice = 10;
                } else {
                    tktPrice = 8;
                }
            }
        }

        return tktPrice;
    }

}
