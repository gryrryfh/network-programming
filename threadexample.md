import java.net.*;
import java.io.*;
import java.nio.file.*;

public class UDPMyEcho {
    final static int MAXBUFFER = 512;

    public static void main(String[] args) {
        if (args.length != 2) {
            System.out.println("how to use : java UDPMyEcho host-ip port");
            System.exit(0);
        }

        byte buffer[] = new byte[MAXBUFFER];
        int bufferLength = 0;
        int port = Integer.parseInt(args[1]);

        try {
            InetAddress inetaddr = InetAddress.getByName(args[0]);
            DatagramSocket Socket = new DatagramSocket();
            DatagramPacket send_packet;
            DatagramPacket recv_packet;

            StringBuilder stringBuilder = new StringBuilder();
            FileWriter fileWriter = new FileWriter("results.txt", true);
            BufferedWriter bufferedWriter = new BufferedWriter(fileWriter);

            while (true) {
                System.out.print("Input Data : ");

                bufferLength = System.in.read(buffer);

                String inputString = new String(buffer, 0, bufferLength).trim();
                stringBuilder.append("input string : ").append(inputString).append('\n');

                send_packet = new DatagramPacket(buffer, buffer.length, inetaddr, port);
                Socket.send(send_packet);

                recv_packet = new DatagramPacket(buffer, buffer.length);
                Socket.receive(recv_packet);

                String result = new String(recv_packet.getData(), 0, recv_packet.getLength());

                String reversedResult = new StringBuilder(result).reverse().toString();
                stringBuilder.append("Reversed Data : ").append(reversedResult).append('\n');
                System.out.println("Reversed Data : " + reversedResult + "\n");


                if (result.indexOf("BYE") >= 0) {
                    break;
                }
            }
            System.out.println("Saving results...");

            bufferedWriter.write(stringBuilder.toString());
            bufferedWriter.newLine();
            bufferedWriter.flush();

            bufferedWriter.close();
            fileWriter.close();

            Socket.close();

        } catch (UnknownHostException ex) {
            System.out.println("Error in the host address ");
        } catch (IOException e) {
            System.out.println(e);
        }
    }
}
