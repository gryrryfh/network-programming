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
```
import java.net.*;
import java.io.*;
import java.nio.file.*;

public class UDPMyEcho {
    final static int MAXBUFFER = 512;

    public static void main(String[] args) {
        if (args.length != 2) {
            System.out.println("사용법: java UDPMyEcho host-ip port");
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

            ReverseThread reverseThread = new ReverseThread();
            reverseThread.start();

            RunnableSaver runnableSaver = new RunnableSaver();
            Thread saverThread = new Thread(runnableSaver);
            saverThread.start();

            while (true) {
                System.out.print("Input Data : ");
                bufferLength = System.in.read(buffer);

                send_packet = new DatagramPacket(buffer, buffer.length, inetaddr, port);
                Socket.send(send_packet);

                recv_packet = new DatagramPacket(buffer, buffer.length);
                Socket.receive(recv_packet);

                String result = new String(buffer, 0, bufferLength);
                System.out.println("Echo Data : " + result);

                String reversedResult = new StringBuilder(result).reverse().toString();
                System.out.println("Reversed Data : " + reversedResult);

                if (result.indexOf("BYE") >= 0)
                    break;
            }
            Socket.close();
        } catch (UnknownHostException ex) {
            System.out.println("Error in the host address ");
        } catch (IOException e) {
            System.out.println(e);
        }
    }

    static class ReverseThread extends Thread {
        @Override
        public void run() {
            // 여기에 거꾸로 된 문자열 출력을 처리할 로직을 추가하세요.
        }
    }

    static class RunnableSaver implements Runnable {
        @Override
        public void run() {
            try {
                // 결과를 저장할 메모장 파일
                String fileName = "results.txt";

                // 메모장에 결과 저장
                FileWriter fileWriter = new FileWriter(fileName, true);
                BufferedWriter bufferedWriter = new BufferedWriter(fileWriter);

                // 여기에 저장할 결과를 쓰는 로직을 추가하세요.

                bufferedWriter.close();
                fileWriter.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```
