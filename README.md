# Mini_Project

~~~ java
import java.util.Scanner;

public class Matrix {

    private double[][] data;
    private int rows, cols;

    public Matrix(int rows, int cols) {
        this.rows = rows;
        this.cols = cols;
        this.data = new double[rows][cols];
    }

    public int getRows() {
        return rows;
    }

    public int getCols() {
        return cols;
    }

    public void readMatrix(Scanner sc) {
        System.out.println("Enter " + rows + "x" + cols + " elements:");
        for (int i = 0; i < rows; i++)
            for (int j = 0; j < cols; j++)
                data[i][j] = sc.nextDouble();
        sc.nextLine();
    }

    public void display() {
        System.out.println();
        for (int i = 0; i < rows; i++) {
            System.out.print("| ");
            for (int j = 0; j < cols; j++)
                System.out.printf("%8.2f ", data[i][j]);
            System.out.println("|");
        }
        System.out.println();
    }

    public Matrix add(Matrix other) {
        if (rows != other.rows || cols != other.cols)
            throw new IllegalArgumentException(
                "Addition requires same dimensions. Got "
                + rows + "x" + cols + " and "
                + other.rows + "x" + other.cols);

        Matrix result = new Matrix(rows, cols);

        for (int i = 0; i < rows; i++)
            for (int j = 0; j < cols; j++)
                result.data[i][j] = data[i][j] + other.data[i][j];

        return result;
    }

    public Matrix subtract(Matrix other) {
        if (rows != other.rows || cols != other.cols)
            throw new IllegalArgumentException(
                "Subtraction requires same dimensions.");

        Matrix result = new Matrix(rows, cols);

        for (int i = 0; i < rows; i++)
            for (int j = 0; j < cols; j++)
                result.data[i][j] = data[i][j] - other.data[i][j];

        return result;
    }

    public Matrix multiply(Matrix other) {
        if (cols != other.rows)
            throw new IllegalArgumentException(
                "Multiply: cols of A (" + cols +
                ") must equal rows of B (" + other.rows + ")");

        Matrix result = new Matrix(rows, other.cols);

        for (int i = 0; i < rows; i++)
            for (int j = 0; j < other.cols; j++)
                for (int k = 0; k < cols; k++)
                    result.data[i][j] += data[i][k] * other.data[k][j];

        return result;
    }

    public Matrix transpose() {
        Matrix result = new Matrix(cols, rows);

        for (int i = 0; i < rows; i++)
            for (int j = 0; j < cols; j++)
                result.data[j][i] = data[i][j];

        return result;
    }

    public double determinant() {
        if (rows != cols)
            throw new IllegalArgumentException(
                "Determinant requires a square matrix.");

        if (rows == 1)
            return data[0][0];

        if (rows == 2)
            return data[0][0] * data[1][1]
                 - data[0][1] * data[1][0];

        if (rows == 3) {
            double[][] d = data;

            return d[0][0] * (d[1][1] * d[2][2] - d[1][2] * d[2][1])
                 - d[0][1] * (d[1][0] * d[2][2] - d[1][2] * d[2][0])
                 + d[0][2] * (d[1][0] * d[2][1] - d[1][1] * d[2][0]);
        }

        throw new UnsupportedOperationException(
            "Determinant supported for 1x1, 2x2, 3x3 only.");
    }

    public Matrix inverse() {
        if (rows != 2 || cols != 2)
            throw new UnsupportedOperationException(
                "Inverse is supported for 2x2 matrices only.");

        double det = determinant();

        if (det == 0)
            throw new ArithmeticException(
                "Singular matrix — inverse does not exist.");

        Matrix result = new Matrix(2, 2);

        result.data[0][0] = data[1][1] / det;
        result.data[0][1] = -data[0][1] / det;
        result.data[1][0] = -data[1][0] / det;
        result.data[1][1] = data[0][0] / det;

        return result;
    }

    public Matrix scalarMultiply(double scalar) {
        Matrix result = new Matrix(rows, cols);

        for (int i = 0; i < rows; i++)
            for (int j = 0; j < cols; j++)
                result.data[i][j] = data[i][j] * scalar;

        return result;
    }
}
~~~

    import java.util.Scanner;

    public class MatrixToolkit {

    static Scanner sc = new Scanner(System.in);

    public static void main(String[] args) {

        System.out.println("==========================================");
        System.out.println(" MATRIX OPERATIONS TOOLKIT (2D) ");
        System.out.println(" Language: Java | SRIT 2025-2026 ");
        System.out.println("==========================================");

        boolean running = true;

        while (running) {
            printMenu();
            System.out.print("Choice: ");

            int choice;
            try {
                choice = Integer.parseInt(sc.nextLine().trim());
            } catch (NumberFormatException e) {
                System.out.println("[!] Enter a number 1-8.");
                continue;
            }

            try {
                switch (choice) {
                    case 1:
                        doAddition();
                        break;
                    case 2:
                        doSubtraction();
                        break;
                    case 3:
                        doMultiplication();
                        break;
                    case 4:
                        doTranspose();
                        break;
                    case 5:
                        doDeterminant();
                        break;
                    case 6:
                        doInverse();
                        break;
                    case 7:
                        doScalarMultiply();
                        break;
                    case 8:
                        System.out.println("Goodbye!");
                        running = false;
                        break;
                    default:
                        System.out.println("[!] Invalid choice.");
                }
            } catch (Exception e) {
                System.out.println("\n[ERROR] " + e.getMessage() + "\n");
            }
        }

        sc.close();
    }
    static void printMenu() {
        System.out.println("\n========== MENU ==========");
        System.out.println("1. Addition (A + B)");
        System.out.println("2. Subtraction (A - B)");
        System.out.println("3. Multiplication (A x B)");
        System.out.println("4. Transpose (A^T)");
        System.out.println("5. Determinant |A|");
        System.out.println("6. Inverse (A^-1) [2x2 only]");
        System.out.println("7. Scalar Multiply (k * A)");
        System.out.println("8. Exit");
        System.out.println("==========================");
    }

    static Matrix readMatrix(String name) {
        System.out.print("Rows for Matrix " + name + ": ");
        int r = Integer.parseInt(sc.nextLine().trim());

    System.out.print("Cols for Matrix " + name + ": ");
        int c = Integer.parseInt(sc.nextLine().trim());

        Matrix m = new Matrix(r, c);
        m.readMatrix(sc);

        System.out.print("Matrix " + name + ":");
        m.display();

        return m;
    }

    static void doAddition() {
        System.out.println("\n--- Matrix Addition (A + B) ---");
        Matrix a = readMatrix("A");
        Matrix b = readMatrix("B");
        System.out.println("Result (A + B):");
        a.add(b).display();
    }

    static void doSubtraction() {
        System.out.println("\n--- Matrix Subtraction (A - B) ---");
        Matrix a = readMatrix("A");
        Matrix b = readMatrix("B");
        System.out.println("Result (A - B):");
        a.subtract(b).display();
    }

    static void doMultiplication() {
        System.out.println("\n--- Matrix Multiplication (A x B) ---");
        Matrix a = readMatrix("A");
        Matrix b = readMatrix("B");
        System.out.println("Result (A x B):");
        a.multiply(b).display();
    }

    static void doTranspose() {
        System.out.println("\n--- Transpose ---");
        Matrix a = readMatrix("A");
        System.out.println("Transpose of A:");
        a.transpose().display();
    }

    static void doDeterminant() {
        System.out.println("\n--- Determinant |A| ---");
        Matrix a = readMatrix("A");
        System.out.printf("Determinant = %.4f%n", a.determinant());
    }

    static void doInverse() {
        System.out.println("\n--- Inverse A^-1 [2x2 only] ---");
        Matrix a = readMatrix("A");
        System.out.println("Inverse of A:");
        a.inverse().display();
    }

    static void doScalarMultiply() {
        System.out.println("\n--- Scalar Multiplication ---");
        Matrix a = readMatrix("A");

        System.out.print("Enter scalar k: ");
        double k = Double.parseDouble(sc.nextLine().trim());

        System.out.println("Result (" + k + " * A):");
        a.scalarMultiply(k).display();
    }

    outputs
