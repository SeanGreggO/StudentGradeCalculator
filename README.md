public class StudentGradeCalculator {

    
    public static void main(String[] args) {
        try {
            String fileName = "grades.txt";
            java.io.File file = new java.io.File(fileName);
            
            // this part check if the file excist
            if (!file.exists()) {
                System.out.println("File '" + fileName + "' not found. Creating sample file...");
                createSampleFile(fileName);
                System.out.println("Sample file created successfully!");
            }
            
            // this part would calculate and print the grade
            double averageGrade = calculateAverageGrade(fileName);
            System.out.println("\nStudent Grades:");
            printStudentGrades(fileName);
            System.out.printf("Average Grade: %.2f\n", averageGrade);
            
        } catch (Exception e) {
            System.out.println("Error: " + e);
            e.printStackTrace();
        }
    }
    
    // this part create the grade.txt  so on and so fort
    public static void createSampleFile(String fileName) throws java.io.IOException {
        java.io.PrintWriter writer = new java.io.PrintWriter(new java.io.FileWriter(fileName));
        writer.println("Alice 85");
        writer.println("Bob 90");
        writer.println("Charlie 75");
        writer.close();
    }
    
    // and this part should calculate the avarage 
    public static double calculateAverageGrade(String fileName) throws java.io.IOException {
        java.io.BufferedReader reader = new java.io.BufferedReader(new java.io.FileReader(fileName));
        String line;
        double sum = 0;
        int count = 0;
        
        while ((line = reader.readLine()) != null) {
            // Skip empty lines
            if (line.trim().isEmpty()) {
                continue;
            }
            
            // Split the line into name and grade
            String[] parts = line.split("\\s+");
            
            // Skip lines  "dont have at least two parts" or "first part Average"
            if (parts.length < 2 || parts[0].equals("Average")) {
                continue;
            }
            
            // Last part(EXTRACT THE GRADE)
            try {
                double grade = Double.parseDouble(parts[parts.length - 1]);
                sum += grade;
                count++;
            } catch (NumberFormatException e) {
                // Skip lines where the grade isn't a valid number
                System.out.println("Warning: Could not parse grade in line: " + line);
            }
        }
        
        reader.close();
        
        if (count == 0) {
            return 0.0;
        }
        
        return sum / count;
    }
    
    //Prints each student's name and grade
    public static void printStudentGrades(String fileName) throws java.io.IOException {
        java.io.BufferedReader reader = new java.io.BufferedReader(new java.io.FileReader(fileName));
        String line;
        
        while ((line = reader.readLine()) != null) {
            if (line.trim().isEmpty()) {
                continue;
            }
            
            // Split the part into "name and grade"
            String[] parts = line.split("\\s+");
            
            // Skip lines  "dont have at least two parts" or "first part Average"
            if (parts.length < 2 || parts[0].equals("Average")) {
                continue;
            }
            
            // This part would extract all the name except for the last part
            StringBuilder name = new StringBuilder();
            for (int i = 0; i < parts.length - 1; i++) {
                name.append(parts[i]);
                if (i < parts.length - 2) {
                    name.append(" ");
                }
            }
            
            // Extract the grade
            String grade = parts[parts.length - 1];
            
            // Pring the name and grade
            System.out.println(name + ": " + grade);
        }
        
        reader.close();
    }
}
