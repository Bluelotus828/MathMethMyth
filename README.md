# MathMethMyth
Math mess my mind like meth


Step 1: Set Up Your Project with Apache Commons Math
Before you begin, ensure you have the Apache Commons Math library included in your project. If you're using Maven, add the following dependency to your pom.xml:


<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-math3</artifactId>
    <version>3.6.1</version> <!-- Check for the latest version -->
</dependency>


Step 2: Multiple Linear Regression in Java
Here's how you might implement multiple linear regression:


import org.apache.commons.math3.stat.regression.OLSMultipleLinearRegression;

public class MultipleLinearRegressionExample {

    public static void performRegression(double[][] x, double[] y) {
        OLSMultipleLinearRegression regression = new OLSMultipleLinearRegression();
        regression.newSampleData(y, x); // y is the dependent variable, x includes the independent variables

        double[] beta = regression.estimateRegressionParameters(); // Estimates the regression parameters (beta coefficients)
        double[] residuals = regression.estimateResiduals(); // Calculates residuals
        double rSquared = regression.calculateRSquared(); // Calculates R-squared value

        System.out.println("Regression Coefficients:");
        for (double b : beta) {
            System.out.println(b);
        }

        System.out.println("R-squared: " + rSquared);
    }

    public static void main(String[] args) {
        double[][] x = {
                {1, 2, 3},
                {1, 3, 4},
                {1, 4, 5},
                {1, 5, 6}
        };
        double[] y = {2, 3, 4, 5};

        performRegression(x, y);
    }
}



Step 3: Calculating Variance Inflation Factor (VIF)
After performing multiple linear regression, you can calculate the VIF for each independent variable as follows:



public static double[] calculateVIF(OLSMultipleLinearRegression regression, double[][] x) {
    double[] vif = new double[x[0].length];

    for (int i = 0; i < x[0].length; i++) {
        double[][] xWithoutI = new double[x.length][x[0].length - 1];
        double[] y = new double[x.length];

        int colIdx = 0;
        for (int j = 0; j < x[0].length; j++) {
            if (j != i) { // Skip the current variable for VIF calculation
                for (int k = 0; k < x.length; k++) {
                    xWithoutI[k][colIdx] = x[k][j];
                }
                colIdx++;
            } else {
                for (int k = 0; k < x.length; k++) {
                    y[k] = x[k][j];
                }
            }
        }

        OLSMultipleLinearRegression vifRegression = new OLSMultipleLinearRegression();
        vifRegression.newSampleData(y, xWithoutI);
        double rSquared = vifRegression.calculateRSquared();
        vif[i] = 1.0 / (1.0 - rSquared);
    }

    return vif;
}

