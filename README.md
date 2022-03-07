import javafx.stage.Stage;
import javafx.application.Application;
import javafx.scene.Group;
import javafx.scene.Scene;
import javafx.scene.shape.Line;
import javafx.scene.shape.*;
import javafx.stage.Stage;
import javafx.scene.paint.Color;
import javafx.collections.*;
import javafx.scene.control.*;
import javafx.scene.image.*;
import java.io.*;
import javafx.scene.layout.*;
import javafx.event.*;
import javafx.scene.input.*;
import javafx.scene.transform.Scale;
import javafx.scene.text.*;
import javafx.geometry.*;
import java.util.*;
import javafx.beans.value.ChangeListener;
import javafx.beans.value.ObservableValue;
import javafx.stage.Modality;

public class Grid extends Application {
	static int clicked = -1;
	static String click1 = new String(), click2 = new String();
	static Button button1 = new Button();
	
	@Override
	public void start(Stage stage) {
		// color array
		Color[] list = new Color[] {Color.BLUE,
									Color.GREEN,
									Color.AQUA,
									Color.RED,
									Color.AQUA,
									Color.GRAY,
									Color.CHOCOLATE,
									Color.BLACK,
									Color.BLUE,
									Color.CHARTREUSE,
									Color.RED,
									Color.BLACK,
									Color.GRAY,
									Color.CHOCOLATE,
									Color.CHARTREUSE,
									Color.GREEN};
		
		// Menu Items
		MenuItem m1 = new MenuItem("New Game");
		MenuItem m2 = new MenuItem("Exit");

		MenuItem s1 = new MenuItem("Increase Font Size");
		MenuItem s2 = new MenuItem("Decrease Font Size");
		
		MenuItem h1 = new MenuItem("How to play");

		// Menu Item events
		EventHandler<ActionEvent> newevent = new EventHandler<ActionEvent>() {
			public void handle(ActionEvent ae) {
				start(stage);
			}
		};
		m1.setOnAction(newevent);
		
		EventHandler<ActionEvent> quitevent = new EventHandler<ActionEvent>() {
			public void handle(ActionEvent ae) {
				// quit
			}
		};
		m2.setOnAction(quitevent);
		
		h1.setOnAction(new EventHandler<ActionEvent>() {
			public void handle(ActionEvent e) {
                h1.setText("Start clicking any box you wish, "
                		+ "\nIf the two consecutive boxes that you click are of same color"
                		+ " then the box along with the number on it dissapears, "
                		+ "\nIf two consective boxes are of different colour then the box resets."
                		+ "\nTo start new game go to file and click on New Game. \nTo quit the game go to file and click quit");
			}
		});

		// Menus
		Menu menu1 = new Menu("File");
		menu1.getItems().addAll(m1, m2);

		Menu options = new Menu("Options");
		options.getItems().addAll(s1, s2);

		Menu help = new Menu("Help");
		help.getItems().add(h1);

		// Menu Bar
		MenuBar mb1 = new MenuBar();
		mb1.getMenus().addAll(menu1, options, help);
		
		//Button Grid
		GridPane grid = new GridPane();
		grid.setPadding(new Insets(10));
		grid.setHgap(10);
		grid.setVgap(10);
		
		int k=0;
		for (int i = 0; i < 4; i++) {
			for (int j = 0; j < 4; j++) {
				Button button = new Button(String.valueOf(k++));
				button.setTranslateX(10);
				button.setTranslateY(10);
				button.setBackground(new Button().getBackground());
				button.setBorder(new Border(new BorderStroke(Color.BLACK, BorderStrokeStyle.SOLID, new CornerRadii(10), BorderWidths.DEFAULT)));
				button.setPrefSize(50, 50);
				
				grid.add(button, i, j);
				
				button.setOnAction(new EventHandler<ActionEvent>() {
					public void handle(ActionEvent ae) {
						int x = Integer.parseInt(button.getText());
						BackgroundFill background_fill = new BackgroundFill(list[x], new CornerRadii(10), Insets.EMPTY);

						// create Background
						Background background = new Background(background_fill);

						// set background
						button.setBackground(background);
						
						clicked++;
						
						if (clicked==0) {
							click1 = button.getBackground().getFills().get(0).toString();
							button1 = (Button) ae.getSource();;
						}
						else if (clicked==1) {
							click2 = button.getBackground().getFills().get(0).toString();
							
							if (click1.equals(click2)) {
								button1.setVisible(false);
								button.setVisible(false);
							}
							else {
								button1.setBackground(new Button().getBackground());
								button1.setBorder(new Border(new BorderStroke(Color.BLACK, BorderStrokeStyle.SOLID, new CornerRadii(10), BorderWidths.DEFAULT)));
								button.setBackground(new Button().getBackground());
								button.setBorder(new Border(new BorderStroke(Color.BLACK, BorderStrokeStyle.SOLID, new CornerRadii(10), BorderWidths.DEFAULT)));
							}
							
							clicked = -1;
							click1 = new String();
							click2 = new String();
						}
					}
				});
			}
		}
		
		// Vbox for menu bar and grid
		VBox vbox = new VBox();
		vbox.getChildren().addAll(mb1, grid);

		Pane root = new Pane(vbox);
		Scene scene = new Scene(root, 260, 290);

		stage.setScene(scene);
		stage.show();
	}

	public static void main(String args[]) {
		launch(args);
	}
}
