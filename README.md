# Lab-3
import java.util.*;
import java.io.*;

public class Main //main class
{
public static void main(String[] args) throws Exception 
{
   int row = 109; //for the amount of artists
   int col = 2;
   String[][] StreamList = new String[row][col];
   read(StreamList);
  
   TopStreamingArtists streams = new TopStreamingArtists();
   for(int r = 0; r < row; r++) 
   {
      streams.insertFirst(StreamList[r][0], StreamList[r][1]);
   }
      streams.AscendingSort();
      streams.displayList();
}
  
public static void read(String[][]arr) throws Exception//reads data from the csv file
{
   BufferedReader br = new BufferedReader(new FileReader("Songs.csv"));
   String note = br.readLine();
   String titles = br.readLine();
     
   String data = br.readLine();
  
   String[] line = data.split(",(?=(?:[^\"]*\"[^\"]*\")*[^\"]*$)");
   arr[0][0] = line[2].trim();
   arr[0][1] = Integer.toString(1);
     
   int rowCount = 1;
   for(int r = 1; r < arr.length+1; r++) 
   {
      data = br.readLine();
      line = data.split(",(?=(?:[^\"]*\"[^\"]*\")*[^\"]*$)");
      if(search(arr, line[2].trim(), r) == 0) 
   {
      arr[rowCount][0] = line[2].trim();
      arr[rowCount][1] = Integer.toString(1);
      rowCount++;
   } 
      else 
   {
      int pos = search(arr, line[2].trim(), r);
      int count = Integer.parseInt(arr[pos][1])+1;
      String c = Integer.toString(count);
      arr[pos][1] = c;
      r = rowCount;
   }
   }
      br.close();
   }
  
public static int search(String[][] arr, String name, int pos) 
{
   for(int r = 0; r < pos; r++) 
   {
      if(name.equals(arr[r][0])) 
      {
         return r;
      }
   }
         return 0;
}
}

      class Artist 
      {
         private String name;
         private String appearances;
         public Artist next;
     
   Artist(String name, String appearances) 
   {
      this.name = name;
      this.appearances = appearances;
   }
  
public void setName(String n) 
{
   name = n;
}
  
public void setAppearances(String a) 
{
   appearances = a;
}
  
public String getName() 
{
   return name;
}
  
public String getAppearances() 
{
   return appearances;
}
  
public String displayArtist()
{
   return(name + ", " + appearances);
}
   }

class TopStreamingArtists 
{
   private Artist first;
  
public TopStreamingArtists() 
{
   first = null;
}
  
public boolean isEmpty() 
{
   return (first == null);
}
  
public void insertFirst(String artistName, String numAppear) 
{
   Artist a = new Artist(artistName, numAppear);
   a.next = first;
   first = a;
}
  
public void AscendingSort() 
{
   Artist current = first;
   Artist next = null;
   String NameTemp;
   String CountTemp;
   if(first == null) 
   {
      return;
   } 
      else 
   {
      while(current != null) 
      {
         next = current.next;   
         while(next != null) 
         {
            if(current.getName().compareToIgnoreCase(next.getName()) > 0) 
            {
               NameTemp = current.getName();
               CountTemp = current.getAppearances();
                 
               current.setName(next.getName());
               current.setAppearances(next.getAppearances());
  
               next.setName(NameTemp);
               next.setAppearances(CountTemp);
            }
               next = next.next;
         }
            current = current.next;
      }
   }
}
  
public void displayList() throws Exception
{
   PrintWriter pw = new PrintWriter("ArtistOutput.txt");
   Artist current = first;
   while(current != null)
   {
      current.displayArtist();
      pw.println(current.displayArtist());
      current = current.next;
    }
      pw.close();
}
}
