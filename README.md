# BWI CHALLANGE

By Prince Mohammad Arsalan Chughtai
princarsalan@live.de

Guten Tag,

Das ist mein Java Code für die BWI Challenge.
Für die Challenge habe ich das Programm BlueJ https://www.bluej.org/ auf Windows benutzt.
Damit Sie den Code ausprobieren können, müssen sie nach dem Herunterladen eine neue Klasse in einem neuen Projekt in BlueJ erstellen. Dann klicken sie doppelt auf die neu erstellte Klasse und fügen dort den Code hinzu. Nach dem Übersetzten bzw. Compilieren sollte es ungefähr so aussehen.

Mit einem Rechtsklick auf die Klasse Wählen sie die erste Methode aus mit dem namen “new NutzwertBerechner(double fahrer1, double fahrer2)”


Dann geben sie die gewichte der beiden Fahrer in Kilo Gramm an und drücken auf ok. Achten Sie dabei darauf das sie keine Kommas verwenden, sondern Punkte.
Danach sollte sich ein Fenster öffnen. Darauf zu sehen ist eine Liste mit der Anzahl der Waren pro Transporter und deren kumulierten Nutzwert.

# Hier die optimale Verteilung, wenn beide Fahrer nur 72,4 kg wiegen:

Der erste Transporter wird mit:
Mobiltelefon Büro  		Anzahl: 1
Mobiltelefon Outdoor  	Anzahl: 27
Mobiltelefon Heavy Duty  	Anzahl: 220
Tablet outdoor klein 		Anzahl: 1
Tablet outdoor groß  		Anzahl: 370

beladen und der zweite mit:
Mobiltelefon Outdoor  	Anzahl: 130
Tablet outdoor klein  		Anzahl: 533

Der optimale Gesamtnutzwert liegt bei 72940.


# Warum dieser Algorithmus?

Ich habe mich für diesen Algorithmus entschieden, weil ich ihn selbst erechnet habe indem ich davor ausprobiert habe wie ich auf den größten Nutzwert komme. Dabei habe ich erkannt worauf ich achten muss und das dann in den Code implementiert. 

# Code

/**
 * Diese Klasse berechnet den größt möglichen Nutzwert der Auslieferung. 
 * Für die Fahrt an den Bonner Standort, mit 2 Transportern und zwei Fahrern mit einem Gewicht zwischen 72.4kg - 85.7kg.
 * Challange der BWI.
 * 
 * @author Prince Mohammad Arsalan Chughtai.
 * @version 2020.12.25
 */

public class NutzwertBerechner
{
    // Hier werden die Namen der Waren gespeichert
    private String [] hardware = new String [10];
    
    // Hier wird das Gewicht der Fahrer in Gramm gespeichert
    private int ersterFahrer;
    private int zweiterFahrer;
    
    // Kapazitäten[0], Gewichte der Waren [1], Nutzwerte [2] und Anzahl der Ware die Transportiert wird [3]
    int [] [] kapazitäten = new int [4] [10];
    
    // Maximal Gewicht von beiden Transportern in g
    private int maxGewichtT1 = 1100000 ;
    private int maxGewichtT2 = 1100000 ;
    
    //Höchste kumulierte Nutzwert
    private int maxNutzwert = 0;
    
    //Momentan Gewicht der Beladung pro Transporter
    private  int momentanGewicht = 0;
    
    //Nummer des Transporters
    private int transporter = 1;
    
    

    /**
     * Berechnet anhand des Gewichts in kg der beiden Fahrer, den größt möglichen Nutzwert für die Fahrt.
     */
    public NutzwertBerechner(double fahrer1, double fahrer2)
    {      
        //Liste der Benötigten Hardware
        detailsHardware();

        //Das Gewicht der beiden Fahrer in mg
        ersterFahrer = (int)(fahrer1 * 1000);
        zweiterFahrer = (int)(fahrer2 * 1000);
        
        //Das maximal Gewicht der Ladung pro Transporter
        maxGewichtT1 -= ersterFahrer ; 
        maxGewichtT2 -= zweiterFahrer ;
        
        //Berechnung vom höchsten Nutzwert pro Transporter
        größterNutzwert(maxGewichtT1);
        größterNutzwert(maxGewichtT2);
        
    }

    
    /**
     * Liste der Benötigten Hardware, Kapazitäten und Nutzwerte
     */
    private void detailsHardware()
    {
        // Die Liste der benötigten Hardware
        hardware[0]="Notebook Büro 13'' " ;
        hardware[1]="Notebook Büro 14'' " ;
        hardware[2]="Notebook outdoor " ;
        hardware[3]="Mobiltelefon Büro " ;
        hardware[4]="Mobiltelefon Outdoor " ;
        hardware[5]="Mobiltelefon Heavy Duty " ;
        hardware[6]="Tablet Büro klein " ;
        hardware[7]="Tablet Büro groß " ;
        hardware[8]="Tablet outdoor klein " ;
        hardware[9]="Tablet outdoor groß " ;
        
        //Liste der benötigten Anzahl Einheiten in Bonn
        kapazitäten[0][0]= 205 ;
        kapazitäten[0][1]= 420 ;
        kapazitäten[0][2]= 450 ;
        kapazitäten[0][3]= 60  ;
        kapazitäten[0][4]= 157 ;
        kapazitäten[0][5]= 220 ;
        kapazitäten[0][6]= 620 ;
        kapazitäten[0][7]= 250 ;
        kapazitäten[0][8]= 540 ;
        kapazitäten[0][9]= 370 ;
        
        //Liste der Gewichte (mit Verpackung und Zubehör), in g
        kapazitäten[1][0]= 2451 ;
        kapazitäten[1][1]= 2978 ;
        kapazitäten[1][2]= 3625 ;
        kapazitäten[1][3]= 717  ;
        kapazitäten[1][4]= 988  ;
        kapazitäten[1][5]= 1220 ;
        kapazitäten[1][6]= 1405 ;
        kapazitäten[1][7]= 1455 ;
        kapazitäten[1][8]= 1690 ;
        kapazitäten[1][9]= 1980 ;
        
        //Liste des Nutzwert je Hardware-Einheit (hoch=besser)
        kapazitäten[2][0]= 40 ;
        kapazitäten[2][1]= 35 ;
        kapazitäten[2][2]= 80 ;
        kapazitäten[2][3]= 30 ;
        kapazitäten[2][4]= 60 ;
        kapazitäten[2][5]= 65 ;
        kapazitäten[2][6]= 40 ;
        kapazitäten[2][7]= 40 ;
        kapazitäten[2][8]= 45 ;
        kapazitäten[2][9]= 68 ;   
        
    }
   
    /**
     * Ermittelt den größten Nutzwert unter allen Nutzwerten.
     */
    private int höchsterNutzwert()
    {
        int höchsterNutzwert = 0;
        //Läuft durch alle Nutzwerte und sucht heraus welcher der größte ist
        for(int i = 0; i<10;i++)
        {
            if (kapazitäten[2][i] > höchsterNutzwert)
            { höchsterNutzwert = kapazitäten[2][i]; }
        }
        
        return höchsterNutzwert;
    }
    
    /**
     * Ermittelt unter den Waren mit dem höchsten Nutzwert, die Ware mit dem kleinstes Gewicht.
     */
    public int kleinstesGewichtVomHöchstenNutzwert()
    {
        //Läuft durch alle Gewichte von den Waren mit dem höchsten Nutzwert durch und sucht heraus welcher das kleinste ist.
        int gewichtVomHöchstenNutzwert = 1100000; //
        for(int i = 0; i<10;i++)
        {
            if (kapazitäten[2][i] == höchsterNutzwert() && gewichtVomHöchstenNutzwert > kapazitäten[1][i])
            { 
                gewichtVomHöchstenNutzwert = kapazitäten[1][i]; 
            }
        }
        
        return gewichtVomHöchstenNutzwert;
    }
    
    /**
     * Erstellt eine Liste der Waren und die anzahl ihrer verladungen im Transporter.
     */
    private void warenListe()
    {
        for(int i = 0; i<10;i++)
        {
            System.out.println(i+1+". "+hardware[i]+" Anzahl: "+kapazitäten[3][i]);
        }
        System.out.println(" ");
    }
    
    /**
     * Erstellt leere Liste der Waren.
     */
    private void leereListe()
    {
        for(int i = 0; i<10;i++)
        {
            kapazitäten[3][i] = 0;
        }
    }

    /**
     * Berechnet den größt möglichen Nutwert für einen Transport.
     */
    public int größterNutzwert(int maxGewichtTransporter){
        momentanGewicht = 0;
        
        //Fügt alle Waren hinzu die weniger wiegen und einen Nutzwert größer als 3/4 haben, als die Ware mit dem höchsten Nutzwert.
        for(int i = 0; i<10;i++)
        {
            if(kapazitäten[2][i]> höchsterNutzwert()/2+höchsterNutzwert()/4 
            && kapazitäten[1][i]< kleinstesGewichtVomHöchstenNutzwert())
            {
                while(momentanGewicht < maxGewichtTransporter && kapazitäten[0][i]!=0)
                {
                    momentanGewicht += kapazitäten[1][i];
                    maxNutzwert += kapazitäten[2][i];
                    kapazitäten[0][i] -= 1;
                    kapazitäten[3][i] += 1;
                }
                if(momentanGewicht > maxGewichtTransporter)
                {
                    momentanGewicht -= kapazitäten[1][i];
                }
            }
        }
        
        //Fügt alle Waren hinzu die weniger als die hälfte wiegen und einen Nutzwert haben der gößer ist als die hälfte von der Ware mit dem höchsten Nutzwert.
        for(int i = 0; i<10;i++)
        {
            if(kapazitäten[2][i]> höchsterNutzwert()/2 
            && kapazitäten[1][i]< kleinstesGewichtVomHöchstenNutzwert()/2)
            {
                while(momentanGewicht < maxGewichtTransporter && kapazitäten[0][i]!=0)
                {
                    momentanGewicht += kapazitäten[1][i];
                    maxNutzwert += kapazitäten[2][i];
                    kapazitäten[0][i] -= 1;
                    kapazitäten[3][i] += 1;
                }
                if(momentanGewicht > maxGewichtTransporter)
                {
                    momentanGewicht -= kapazitäten[1][i];
                }
            }
        }
         
        //Aktulasiert das gewicht nach der beladung.
        maxGewichtTransporter -= momentanGewicht;
        
        //Füllt den freien Platz mit passender Ware
        restGewicht(maxGewichtTransporter);
        
        //Erstell eine Liste pro Transporter
        warenListe();
        leereListe();        
        
        return maxNutzwert;
    } 
    
    /**
     * Schaut sich den platz an der im Transporter bleibt und füllt ihn mit waren.
     */
    public void restGewicht(int maxGewichtTransporter)
    {
        while(maxGewichtTransporter>700)
        {
            for(int i = 0; i<10;i++)
            {
                if (kapazitäten[1][i] < maxGewichtTransporter && kapazitäten[0][i]!=0)
                { 
                    momentanGewicht += kapazitäten[1][i];
                    maxNutzwert += kapazitäten[2][i];
                    kapazitäten[0][i] -= 1;
                    kapazitäten[3][i] += 1;
                    maxGewichtTransporter -= kapazitäten[1][i];
                }
    
            }
        }
        
        // Gibt das Restgewicht an, welches im Transporter frei ist und wie hoch der gesamte Nutzwert ist.
        System.out.println("Das Gewicht des "+transporter+". Transportes, liegt bei "+momentanGewicht+"g und es sind noch "+maxGewichtTransporter+"g frei .");
        System.out.println("Der Gesamtnutzwert bis jetzt liegt bei "+maxNutzwert);
        System.out.println(" ");
        
        transporter++;
    }
    
    /**
     * Sortiert die Liste nach Nutzwerten.
     */
    public void sortierenNachNutzwert()
    {
        for(int k = 0; k<hardware.length;k++)
        {
            for(int i = 0; i<hardware.length - 1;i++)
            {
                if(kapazitäten[2][i]<kapazitäten[2][i+1])
                { 
                    int nutzwertTausch = kapazitäten[2][i];
                    kapazitäten[2][i] = kapazitäten[2][i+1];
                    kapazitäten[2][i+1] = nutzwertTausch;
                    
                    int gewichtTausch = kapazitäten[1][i];
                    kapazitäten[1][i] = kapazitäten[1][i+1];
                    kapazitäten[1][i+1] = gewichtTausch;
                    
                    int anzahlTausch = kapazitäten[0][i];
                    kapazitäten[0][i] = kapazitäten[0][i+1];
                    kapazitäten[0][i+1] = anzahlTausch;
                    
                    String name = hardware[i];
                    hardware[i] = hardware[i+1];
                    hardware[i+1] = name;
                }
            }
        }
    }
    
    /**
     * Sortiert die Liste nach Gewicht.
     */
    public void sortierenNachGewicht()
    {
        for(int k = 0; k<hardware.length;k++)
        {
            for(int i = 0; i<hardware.length - 1;i++)
            {
                if(kapazitäten[1][i]>kapazitäten[1][i+1])
                { 
                    int nutzwertTausch = kapazitäten[2][i];
                    kapazitäten[2][i] = kapazitäten[2][i+1];
                    kapazitäten[2][i+1] = nutzwertTausch;
                    
                    int gewichtTausch = kapazitäten[1][i];
                    kapazitäten[1][i] = kapazitäten[1][i+1];
                    kapazitäten[1][i+1] = gewichtTausch;
                    
                    int anzahlTausch = kapazitäten[0][i];
                    kapazitäten[0][i] = kapazitäten[0][i+1];
                    kapazitäten[0][i+1] = anzahlTausch;
                    
                    String name = hardware[i];
                    hardware[i] = hardware[i+1];
                    hardware[i+1] = name;
                }
            }
        }
    }
}
