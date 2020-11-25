#局域网Ip检测应用.md

方法一：ping方式 
---

````
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;
import java.lang.Process;
import java.lang.Runtime;
import java.io.IOException;

public class Ping {
    public static void main(String[] args) {
        StringBuilder ping_command = new StringBuilder();
        StringBuilder output = new StringBuilder();
        StringBuilder ip_output = new StringBuilder();
        List<String> live_ip = new ArrayList<String>();

        final String IP = new String("192.168.");
        final String CMD = new String("ping ");
        final String CMDARG = new String(" -n 1");
        short NUM3 = 0;
        short NUM4 = 0;
        short NUM4_start = 0;
        short NUM4_end = 255;



        ping_command.append(CMD).append(IP);

        System.out.println("Finding local IP's that can be talked to...\n");
        Scanner scnr = new Scanner(System.in);
        System.out.println("What value for the third set of bytes(192.168.???.xxx) (0-255): ");
        short ans1 = scnr.nextShort();
        scnr.nextLine();
        System.out.println("Would you like to multiple values for the fourth set of bytes(192.168.xxx.???) (y/n): ");
        String ans2 = scnr.nextLine();

        if (ans1 >= 0 && ans1 <= 255) {
            NUM3 = ans1;
        }

        if (ans2.equalsIgnoreCase("y")) {
            System.out.println("Start Position: ");
            short ans3 = scnr.nextShort();
            System.out.println("End Position: ");
            short ans4 = scnr.nextShort();

            NUM4_start = ans3;
            NUM4_end = ans4;
            NUM4 = NUM4_start;
        }

        scnr.close();

        ping_command.append(NUM3).append(".");

        try {
            Runtime r = Runtime.getRuntime();
            Process p;
            BufferedReader in;

            while (NUM4 >= 0 && NUM4 <= NUM4_end) {
                ping_command.append(NUM4).append(CMDARG);
                p = r.exec(ping_command.toString());
                in = new BufferedReader(new InputStreamReader(p.getInputStream()));
                String inputLine;

                while((inputLine = in.readLine()) != null) {
                    output.append(inputLine).append("\n");
                }
                if (output.toString().contains("time")) {
                    ip_output.delete(0, ip_output.length());
                    ip_output.append(IP + NUM4);
                    live_ip.add(ip_output.toString());
                }

                NUM4++;
                ping_command.delete(17, ping_command.length());
                output.delete(0, output.length());
            }

        } catch (IOException e) {
            e.printStackTrace();
        }

        live_ip.forEach(System.out::println);
    }
}






````

