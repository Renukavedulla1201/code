import java.io.File;
import java.util.*;

public class ShamirSecretSharing {

    // Function to decode the y-value
    public static long decodeValue(int base, String value) {
        return Long.parseLong(value, base);
    }

    // Function to perform Lagrange interpolation and find the constant term (f(0))
    public static double lagrangeInterpolation(List<double[]> points) {
        int k = points.size();
        double result = 0.0;

        for (int i = 0; i < k; i++) {
            double term = points.get(i)[1]; // y_i
            for (int j = 0; j < k; j++) {
                if (i != j) {
                    term *= (0 - points.get(j)[0]) / (points.get(i)[0] - points.get(j)[0]); // x = 0
                }
            }
            result += term;
        }
        return result;
    }

    // Manual JSON parsing function
    public static Map<String, Object> parseJson(String fileName) throws Exception {
        Map<String, Object> result = new HashMap<>();
        Scanner sc = new Scanner(new File(fileName));
        sc.useDelimiter("\\Z");
        String content = sc.next();
        content = content.replaceAll("[{}\"]", ""); // Simplistic JSON parsing
        String[] entries = content.split(",");
        for (String entry : entries) {
            String[] keyValue = entry.split(":");
            result.put(keyValue[0].trim(), keyValue[1].trim());
        }
        sc.close();
        return result;
    }

    // Function to solve the polynomial for a given JSON test case
    public static double solvePolynomial(String fileName) {
        try {
            Map<String, Object> data = parseJson(fileName);

            // Read keys n and k
            Map<String, Long> keys = new HashMap<>();
            keys.put("n", Long.parseLong(data.get("keys.n").toString()));
            keys.put("k", Long.parseLong(data.get("keys.k").toString()));

            long n = keys.get("n");
            long k = keys.get("k");

            // Extract the roots as points (x, y)
            List<double[]> points = new ArrayList<>();
            for (String key : data.keySet()) {
                if (key.startsWith("keys")) continue;

                int x = Integer.parseInt(key);
                String baseValue = data.get(key + ".base").toString();
                int base = Integer.parseInt(baseValue);
                String encodedValue = data.get(key + ".value").toString();
                long y = decodeValue(base, encodedValue);

                points.add(new double[]{x, y});
            }

            // Use the first k points to calculate the constant term
            return lagrangeInterpolation(points.subList(0, (int) k));

        } catch (Exception e) {
            e.printStackTrace();
        }
        return -1;
    }

    public static void main(String[] args) {
        // Solve both test cases
        double secret1 = solvePolynomial("testcase1.json");
        double secret2 = solvePolynomial("testcase2.json");

        System.out.println("Secret for Test Case 1: " + Math.round(secret1));
        System.out.println("Secret for Test Case 2: " + Math.round(secret2));
    }
}
