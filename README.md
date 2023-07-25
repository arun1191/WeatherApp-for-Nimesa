# WeatherApp-for-Nimesa
package Practice;
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;
import java.util.Scanner;

public class WeatherApp {

    // Replace 'YOUR_API_KEY' with the actual API key for the weather service
    private static final String API_KEY = "YOUR_API_KEY";
    private static final String BASE_URL = "https://api.example.com/weather"; // Replace with the actual API URL

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        while (true) {
            System.out.println("\nOptions:");
            System.out.println("1. Get temperature");
            System.out.println("2. Get wind speed");
            System.out.println("3. Get pressure");
            System.out.println("0. Terminate program");

            int option = scanner.nextInt();
            scanner.nextLine(); // Consume the newline character after reading the integer option

            if (option == 0) {
                System.out.println("Terminating program.");
                break;
            } else if (option == 1) {
                printTemperature(scanner);
            } else if (option == 2) {
                printWindSpeed(scanner);
            } else if (option == 3) {
                printPressure(scanner);
            } else {
                System.out.println("Invalid option. Please choose again.");
            }
        }

        scanner.close();
    }

    private static void printTemperature(Scanner scanner) {
        System.out.print("Enter the date (yyyy-mm-dd): ");
        String date = scanner.nextLine();
        String weatherData = getWeatherData(date);

        if (weatherData != null) {
            int temperature = extractTemperature(weatherData);
            System.out.println("Temperature on " + date + ": " + temperature + "°C");
        }
    }

    private static void printWindSpeed(Scanner scanner) {
        System.out.print("Enter the date (yyyy-mm-dd): ");
        String date = scanner.nextLine();
        String weatherData = getWeatherData(date);

        if (weatherData != null) {
            int windSpeed = extractWindSpeed(weatherData);
            System.out.println("Wind speed on " + date + ": " + windSpeed + " m/s");
        }
    }
    private static void printPressure(Scanner scanner) {
        System.out.print("Enter the date (yyyy-mm-dd): ");
        String date = scanner.nextLine();
        String weatherData = getWeatherData(date);

        if (weatherData != null) {
            int pressure = extractPressure(weatherData);
            System.out.println("Pressure on " + date + ": " + pressure + " hPa");
        }
    }

    private static String getWeatherData(String date) {
        try {
            URL url = new URL(BASE_URL + "?api_key=" + API_KEY + "&location=London&date=" + date);
            HttpURLConnection connection = (HttpURLConnection) url.openConnection();
            connection.setRequestMethod("GET");

            int responseCode = connection.getResponseCode();
            if (responseCode == 200) {
                BufferedReader reader = new BufferedReader(new InputStreamReader(connection.getInputStream()));
                StringBuilder response = new StringBuilder();
                String line;
                while ((line = reader.readLine()) != null) {
                    response.append(line);
                }
                reader.close();
                return response.toString();
            } else {
                System.out.println("Failed to retrieve weather data for " + date + ".");
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
        return null;
    }

    // Assuming the JSON response contains fields like 'temperature', 'wind_speed', 'pressure'
    private static int extractTemperature(String weatherData) {
        // Implement JSON parsing logic to extract the temperature value from the response
        // For simplicity, let's assume the JSON structure is like: {"temperature": 25, ...}
        // Parse the JSON and extract the temperature value
        // You can use libraries like Gson or Jackson for more complex JSON parsing.
        return 25; // Replace with the actual extracted temperature value
    }

    private static int extractWindSpeed(String weatherData) {
        // Implement JSON parsing logic to extract the wind speed value from the response
        return 8; // Replace with the actual extracted wind speed value
    }

    private static int extractPressure(String weatherData) {
        // Implement JSON parsing logic to extract the pressure value from the response
        return 100; // Replace with the actual extracted pressure value
    }
}
