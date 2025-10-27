import java.util.Scanner;
class Stack 
{
    String[] disks;
    int top;
    int size;

    Stack(int size) 
    {
        this.size = size;
        disks = new String[size];
        top = -1;
    }

    void push(String color) 
    {
        if (top == size - 1) {
            System.out.println("Stack Overflow!");
            return;
        }
        disks[++top] = color;
    }

    String pop() 
    {
        if (top == -1) {
            System.out.println("Stack Underflow!");
            return null;
        }
        return disks[top--];
    }

    String peek() 
    {
        if (top == -1) {
            System.out.println("Stack Empty!");
            return null;
        }
        return disks[top];
    }

    boolean isEmpty() {
        return top == -1;
    }

    int height() {
        return top + 1;
    }

    void display() 
    {
        if (top == -1) {
            System.out.println("Empty Rod");
            return;
        }
        for (int i = top; i >= 0; i--) {
            System.out.print(disks[i] + " ");
        }
        System.out.println();
    }

    //Stack Data Structure Simulation 
    void moveDisk(Stack dest) {
        if (this.top == -1) {
            System.out.println("No disk to move!");
            return;
        }
        String disk = this.pop();
        dest.push(disk);
        System.out.println("Moved disk " + disk + " from one rod to another.");
    }

    //Tower of Hanoi 
    void towerOfHanoi(int n, Stack source, Stack aux, Stack dest) 
    {
        if (n > source.height()) {
            System.out.println("Not enough disks in source rod!");
            return;
        }
        if (n == 1) {
            String disk = source.pop();
            dest.push(disk);
            System.out.println("Move disk " + disk + " from Source to Destination");
            return;
        }
        towerOfHanoi(n - 1, source, dest, aux);
        String disk = source.pop();
        dest.push(disk);
        System.out.println("Move disk " + disk + " from Source to Destination");
        towerOfHanoi(n - 1, aux, source, dest);
    }

    //Sorting Challenge (Sort by color)
    void sortDisks() 
    {
        if (isEmpty()) {
            System.out.println("No disks to sort!");
            return;
        }
        Stack redStack = new Stack(size);
        Stack greenStack = new Stack(size);
        Stack blueStack = new Stack(size);
        while (!isEmpty()) {
            String disk = pop();
            if (disk.equalsIgnoreCase("Red"))
                redStack.push(disk);
            else if (disk.equalsIgnoreCase("Green"))
                greenStack.push(disk);
            else if (disk.equalsIgnoreCase("Blue"))
                blueStack.push(disk);
            else
                System.out.println("Unknown color found: " + disk);
        }
        while (!blueStack.isEmpty())
            push(blueStack.pop());
        while (!greenStack.isEmpty())
            push(greenStack.pop());
        while (!redStack.isEmpty())
            push(redStack.pop());

        System.out.println("Disks sorted by color (Red->Green->Blue)");
    }

    //Color Counting
    static void colorCounting(Stack[] rods) 
    {
        int red = 0, green = 0, blue = 0;
        int maxUnique = 0, rodNum = -1;

        for (int i = 0; i < rods.length; i++) {
            boolean hasRed = false, hasGreen = false, hasBlue = false;
            for (int j = 0; j <= rods[i].top; j++) {
                if (rods[i].disks[j].equalsIgnoreCase("Red")) {
                    red++;
                    hasRed = true;
                } else if (rods[i].disks[j].equalsIgnoreCase("Green")) {
                    green++;
                    hasGreen = true;
                } else if (rods[i].disks[j].equalsIgnoreCase("Blue")) {
                    blue++;
                    hasBlue = true;
                }
            }
            int unique = (hasRed ? 1 : 0) + (hasGreen ? 1 : 0) + (hasBlue ? 1 : 0);
            if (unique > maxUnique) {
                maxUnique = unique;
                rodNum = i + 1;
            }
        }
        System.out.println("Total Red: " + red + ", Green: " + green + ", Blue: " + blue);
        System.out.println("Rod with max unique colors: Rod " + rodNum);
    }

    //Queue Simulation (Turn-based)
    static void queueSimulation(Stack[] rods) 
    {
        Scanner sc = new Scanner(System.in);
        System.out.print("Enter number of turns: ");
        int turns = sc.nextInt();
        for (int i = 0; i < turns; i++) {
            int rodIndex = i % rods.length;
            System.out.print("Enter color for turn " + (i + 1) + ": ");
            String color = sc.next();
            rods[rodIndex].push(color);
        }
        System.out.println("Final configuration after queue simulation:");
        for (int i = 0; i < rods.length; i++) {
            System.out.print("Rod " + (i + 1) + ": ");
            rods[i].display();
        }
    }

    //Balanced Stack Condition
    static void checkBalanced(Stack[] rods)
    {
        int min = rods[0].height();
        int max = rods[0].height();
        for (Stack rod : rods) {
            int h = rod.height();
            if (h < min) min = h;
            if (h > max) max = h;
        }
        if (min == max)
            System.out.println("All rods are balanced!");
        else
            System.out.println("Not balanced! Minimum moves to balance: " + (max - min));
    }

    //Pattern Formation Game
    void formPattern(String[] target) 
    {
        if (top + 1 < target.length) {
            System.out.println("Not enough disks to form pattern!");
            return;
        }
        boolean match = true;
        for (int i = 0; i < target.length; i++) {
            if (!disks[top - i].equalsIgnoreCase(target[i])) {
                match = false;
                break;
            }
        }
        if (match)
            System.out.println("Pattern already formed!");
        else
            System.out.println("Simulate moves to achieve pattern!");
    }

    //Maximum Height Rod
    static void findMaxHeight(Stack[] rods, int K) 
    {
        int max = 0, rodNum = -1;
        for (int i = 0; i < rods.length; i++) {
            if (rods[i].height() > max) {
                max = rods[i].height();
                rodNum = i + 1;
            }
        }
        System.out.println("Rod " + rodNum + " has maximum height: " + max);
        if (max > K)
            System.out.println("Overflow: Rod " + rodNum + " exceeded limit " + K);
    }

    //Probability Scenario
    static void probability(Stack[] rods, String color) 
    {
        int total = 0, colorCount = 0;
        for (Stack rod : rods) {
            total += rod.height();
            for (int i = 0; i <= rod.top; i++) {
                if (rod.disks[i].equalsIgnoreCase(color))
                    colorCount++;
            }
        }
        if (total == 0) {
            System.out.println("No disks to calculate probability!");
            return;
        }
        double probability = (double) colorCount / total;
        System.out.println("Probability of picking " + color + " = " + probability);
    }
}

public class MainStack1 
{
    public static void main(String[] args) 
    {
        Scanner sc = new Scanner(System.in);
        Stack[] rods = {new Stack(10), new Stack(10), new Stack(10)};
        int choice;

        do {
            System.out.println("\n--- Disk Simulation Menu ---");
            System.out.println("0. Push Disk to Rod");
            System.out.println("1. Stack Simulation (Move Disks)");
            System.out.println("2. Tower of Hanoi");
            System.out.println("3. Sort Disks by Color");
            System.out.println("4. Color Counting");
            System.out.println("5. Queue Simulation");
            System.out.println("6. Check Balanced Rods");
            System.out.println("7. Pattern Formation");
            System.out.println("8. Maximum Height Rod");
            System.out.println("9. Probability Scenario");
            System.out.println("10. Display All Rods");
            System.out.println("11. Exit");
            System.out.print("Enter your choice: ");
            choice = sc.nextInt();

            switch (choice) 
            {
                case 0:
                    System.out.print("Enter rod number (1-3): ");
                    int rodNum = sc.nextInt();
                    System.out.print("Enter disk color: ");
                    String colorInput = sc.next();
                    rods[rodNum - 1].push(colorInput);
                    System.out.println("Disk pushed successfully!");
                    break;

                case 1:
                    System.out.print("From rod (1-3): ");
                    int from = sc.nextInt();
                    System.out.print("To rod (1-3): ");
                    int to = sc.nextInt();
                    rods[from - 1].moveDisk(rods[to - 1]);
                    break;

                case 2:
                    System.out.print("Enter number of disks: ");
                    int n = sc.nextInt();
                    rods[0].towerOfHanoi(n, rods[0], rods[1], rods[2]);
                    break;

                case 3:
                    System.out.print("Enter rod to sort (1-3): ");
                    int sortRod = sc.nextInt();
                    rods[sortRod - 1].sortDisks();
                    break;

                case 4:
                    Stack.colorCounting(rods);
                    break;

                case 5:
                    Stack.queueSimulation(rods);
                    break;

                case 6:
                    Stack.checkBalanced(rods);
                    break;

                case 7:
                    System.out.print("Enter pattern length: ");
                    int len = sc.nextInt();
                    String[] pattern = new String[len];
                    for (int i = 0; i < len; i++) {
                        System.out.print("Enter color " + (i + 1) + ": ");
                        pattern[i] = sc.next();
                    }
                    System.out.print("Enter rod number to check: ");
                    int rodP = sc.nextInt();
                    rods[rodP - 1].formPattern(pattern);
                    break;

                case 8:
                    System.out.print("Enter height limit K: ");
                    int K = sc.nextInt();
                    Stack.findMaxHeight(rods, K);
                    break;

                case 9:
                    System.out.print("Enter color to find probability: ");
                    String c = sc.next();
                    Stack.probability(rods, c);
                    break;

                case 10:
                    for (int i = 0; i < 3; i++) {
                        System.out.print("Rod " + (i + 1) + ": ");
                        rods[i].display();
                    }
                    break;

                case 11:
                    System.out.println("Exiting program...");
                    break;

                default:
                    System.out.println("Invalid choice!");
            }
        } while (choice != 11);
    }
}
