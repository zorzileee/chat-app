# chat-app
simple chat app written in java.

public class Server {	
	
	static ArrayList<ServerskaNit> listaKlijenata = new ArrayList<>();
	static ArrayList<String> imenaKlijenata = new ArrayList<>();
	static int udpPort = 20000;
	static final String PORTFILE = "C:\\Users\\Zoran\\workspace\\zokiServer\\zajednicki.txt";
	static PrintWriter outFile ;
	
	public static void main(String[] args) {
		int port = 8090;
		
		if(args.length>0) {
			port = Integer.parseInt(args[0]);
		}
		
		ServerSocket vrataDobrodoslice = null;
		
		try {
			outFile = new PrintWriter(new BufferedWriter(new FileWriter(PORTFILE)));
			vrataDobrodoslice = new ServerSocket(port);
			while(true) {
				Socket connectionSocket = vrataDobrodoslice.accept();
				ServerskaNit noviKlijent = new ServerskaNit(connectionSocket, udpPort);
				udpPort++;
				listaKlijenata.add(noviKlijent);
				noviKlijent.myStart();
			}
		} catch(IOException e) {
			System.err.println("Greska!"+e);
		}

	}
	public synchronized static void welcomeSignals(ServerskaNit s) {
		if(s.getPol().startsWith("m")) {
			s.izlazniTokKaKlijentu.println("** Dobrodosao, "+s.getIme()+". Za izlazak unesite !quit. Tvoj broj udp porta: "+s.udpPort);
		} else {
			s.izlazniTokKaKlijentu.println("** Dobrodosla, "+s.getIme()+". Za izlazak unesite !quit. Tvoj broj udp porta: "+s.udpPort);
		}
		for(ServerskaNit ser : Server.listaKlijenata) {
			if(ser!=null && ser.getIme()!=null && ser.getPol()!=null && !(ser.getPol().equalsIgnoreCase(s.getPol()))) {
				if(s.getPol().startsWith("m")) {
					ser.izlazniTokKaKlijentu.println("** Novi korisnik "+s.getIme()+" je usao u chat sobu.");
				} else {
					ser.izlazniTokKaKlijentu.println("** Nova korisnica "+s.getIme()+" je usla u chat sobu.");
				}
			}
		}
	}
	public synchronized static void writeInFile1(String s1, String s2) {
		//try {
			//outFile = new PrintWriter(new BufferedWriter(new FileWriter(PORTFILE)));
			outFile.println(s1+" salje: "+s2+", vreme: "+(new GregorianCalendar()).getTime());
			//outFile.write('\n');
			//outFile.close();
		/*} catch (IOException e) {
			// TODO Auto-generated catch block
		}*/
		
	}
	public synchronized static void writeInFile2(String s1, String s2) {
		//try {
			//outFile = new PrintWriter(new BufferedWriter(new FileWriter(PORTFILE)));
			outFile.println("Poruka je uspesno poslata: "+s1);
			//outFile.write('\n');
			outFile.println("Poruka nije uspesno poslata: "+s2);
			//outFile.write('\n');
			//outFile.close();
		/*} catch (IOException e) {
			// TODO Auto-generated catch block
		}*/
		
	}


}
