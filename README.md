# Online-Examination

import java.util.*;

class User {
    String username, password;
    User(String u, String p) {
        username = u;
        password = p;
    }
}

public class Main {
    static Scanner sc = new Scanner(System.in);
    static User user = new User("student", "1234");
    static int score = 0;
    static boolean examCompleted = false;

    static String[] questions = {
        "1) Which company developed Java?\n a) Apple\n b) Microsoft\n c) Sun Microsystems\n d) Google",
        "2) Which of these is not a Java keyword?\n a) static\n b) Boolean\n c) void\n d) new",
        "3) What is the capital of India?\n a) Delhi\n b) Mumbai\n c) Kolkata\n d) Chennai",
        "4) Which extensions are used for Java files? (Select all that apply)\n a) .java\n b) .txt\n c) .class\n d) .exe",
        "5) Which loop is guaranteed to execute at least once?\n a) for\n b) while\n c) do-while\n d) foreach"
    };

    static String[] correctAnswers = {
        "c",    // single correct answer
        "b",    // single correct answer
        "a",    // single correct answer
        "bc",   // multiple correct answers
        "c"     // single correct answer
    };

    public static void main(String[] args) {
        if (!login()) return;
        updateProfile();
        System.out.println("\n=== Exam Starts ===");
        exam();
        logout();
    }

    static boolean login() {
        System.out.print("Enter username: ");
        String u = sc.nextLine();
        System.out.print("Enter password: ");
        String p = sc.nextLine();
        if (u.equals(user.username) && p.equals(user.password)) return true;
        System.out.println("Login failed.");
        return false;
    }

    static void updateProfile() {
        System.out.print("\nDo you want to update your profile? (y/n): ");
        if (sc.nextLine().equalsIgnoreCase("y")) {
            System.out.print("Enter new username: ");
            user.username = sc.nextLine();
            System.out.print("Enter new password: ");
            user.password = sc.nextLine();
            System.out.println("Profile updated successfully.");
        }
    }

    static void exam() {
        Timer timer = new Timer();
        timer.schedule(new TimerTask() {
            public void run() {
                if (!examCompleted) {
                    System.out.println("\nTime's up! Auto-submitting...");
                    showScore();
                    System.out.println("Session closed. Logged out due to timeout.");
                    System.exit(0);
                }
            }
        }, 60000); // 1 minute

        for (int i = 0; i < questions.length; i++) {
            System.out.println(questions[i]);
            if (i == 3) { // Question 4 allows multiple answers
                System.out.print("Answer (you may enter multiple options): ");
            } else {
                System.out.print("Answer: ");
            }
            String input = sc.nextLine().toLowerCase().replaceAll("[^a-d]", "");
            if (checkAnswer(input, correctAnswers[i])) {
                score++;
            }
        }

        examCompleted = true;
        timer.cancel();
    }

    static boolean checkAnswer(String input, String correct) {
        char[] inArr = input.toCharArray();
        char[] correctArr = correct.toCharArray();
        Arrays.sort(inArr);
        Arrays.sort(correctArr);
        return Arrays.equals(inArr, correctArr);
    }

    static void showScore() {
        System.out.println("\nYour Score: " + score + "/" + questions.length);
    }

    static void logout() {
        showScore();
        System.out.println("Session closed. Logged out successfully.");
    }
}
