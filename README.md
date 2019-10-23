# GUI-Design
User Interface

    Author: Luc Nguyen
    Class: CEN 3024C
    Profesor: Gossai

    package application;
    import java.io.BufferedReader;
    import java.io.FileReader;
    import java.io.IOException;
    import java.util.ArrayList;
    import java.util.Collections;
    import java.util.Comparator;
    import java.util.HashMap;
    import java.util.List;
    import java.util.Map.Entry;
    import java.util.Set;

    import javafx.application.Application;
    import javafx.application.Platform;
    import javafx.event.ActionEvent;
    import javafx.event.EventHandler;
    import javafx.scene.Scene;
    import javafx.scene.control.Button;
    import javafx.scene.layout.StackPane;
    import javafx.stage.Stage;

    public class textAnalyze extends Application implements EventHandler<ActionEvent> {

	Button button;
	Stage window;

	public static void main(String[] args) {
		launch(args);
	}
	@Override
	public void start(Stage Stage) throws Exception {

		// Tile of the panel and button
		Stage.setTitle("Text Analyzer");
		button = new Button();
		button.setText("Analyze text file");

		// This class will handle the button events
		button.setOnAction(this);

		// Panel layout
		StackPane layout = new StackPane();
		layout.getChildren().add(button);
		Scene scene = new Scene(layout, 300, 250);

		Stage.setScene(scene);
		Stage.show();
	}

	// Exit Application
	public void exitApplication(ActionEvent event) {
		Platform.exit();
	}
	
	public void stop() {
		System.out.println();
		System.out.println("Stage is closed");
		// Save file
		}
	
	// When button is clicked, handle() gets called
	public void handle(ActionEvent event) {

		// created HashMap for counting words & values
		HashMap<String, Integer> wordCountMap = new HashMap<String, Integer>();
		BufferedReader reader = null;

		try {
			// created BufferedReader
			reader = new BufferedReader(new FileReader("C:\\Users\\Student\\Desktop\\Shakespear.txt."));

			// Reading the first line into currentLine
			String currentLine = reader.readLine();
			while (currentLine != null) {
				// splitting the currentLine into words
				String[] words = currentLine.toLowerCase().split(" ");

				// Iterating each word
				for (String word : words) {
					// If word is already in wordCountMap, updated count
					if (wordCountMap.containsKey(word)) {
						wordCountMap.put(word, wordCountMap.get(word) + 1);
					}
					// otherwise inserting the word as key and 1 as its value
					else {
						wordCountMap.put(word, 1);
					}
				}
				// Reading next line into currentLine
				currentLine = reader.readLine();
			}
			// Getting entries of wordCountMapn into form of set
			Set<Entry<String, Integer>> entrySet = wordCountMap.entrySet();

			// Creating a List passing the entrySet
			List<Entry<String, Integer>> list = new ArrayList<Entry<String, Integer>>(entrySet);

			// Sorting the list in decreasing order of values
			Collections.sort(list, new Comparator<Entry<String, Integer>>() {
				@Override
				public int compare(Entry<String, Integer> e1, Entry<String, Integer> e2) {
					return (e2.getValue().compareTo(e1.getValue()));
				}
			});

			// Printing the repeated words in input file along with their occurrences
			System.out.println("Repeated Words In Input File Are :");
			for (Entry<String, Integer> entry : list) {
				if (event.getSource() == button) {
					System.out.println(entry.getKey() + " : " + entry.getValue());
				}
			}
		} catch (IOException e) {
			e.printStackTrace();
		} finally {
			try {
				// Closing the reader
				reader.close();
			} catch (IOException e) {
				e.printStackTrace();
		    	}
	  	   }
	     }
     }
