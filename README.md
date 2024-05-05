import java.util.Arrays;

public class RegressionCalculator {
    
    public static void main(String[] args) {
        // Sample data
        double[] X = {1, 2, 3, 4, 5}; // Independent variable
        double[] Y = {2, 3, 4, 5, 6}; // Dependent variable
        
        // Fit the model
        double[] coefficients = linearRegression(X, Y);
        
        // Print results
        System.out.println("Regression Equation: Y = " + coefficients[1] + "X + " + coefficients[0]);
        double rSquared = calculateRSquared(X, Y, coefficients);
        System.out.println("R-squared: " + rSquared);
    }
    
    // Function to perform linear regression
    public static double[] linearRegression(double[] X, double[] Y) {
        int n = X.length;
        
        // Calculate mean of X and Y
        double meanX = Arrays.stream(X).sum() / n;
        double meanY = Arrays.stream(Y).sum() / n;
        
        // Calculate slope (m)
        double numerator = 0;
        double denominator = 0;
        for (int i = 0; i < n; i++) {
            numerator += (X[i] - meanX) * (Y[i] - meanY);
            denominator += Math.pow((X[i] - meanX), 2);
        }
        double slope = numerator / denominator;
        
        // Calculate intercept (c)
        double intercept = meanY - slope * meanX;
        
        return new double[]{intercept, slope};
    }
    
    // Function to calculate R-squared
    public static double calculateRSquared(double[] X, double[] Y, double[] coefficients) {
        double intercept = coefficients[0];
        double slope = coefficients[1];
        
        double meanY = Arrays.stream(Y).sum() / Y.length;
        
        double ssTotal = 0;
        double ssResidual = 0;
        for (int i = 0; i < X.length; i++) {
            double yPred = intercept + slope * X[i];
            ssTotal += Math.pow((Y[i] - meanY), 2);
            ssResidual += Math.pow((Y[i] - yPred), 2);
        }
        
        return 1 - (ssResidual / ssTotal);
    }
}
