// Brock Stoops
// 20 April 2012
// Sierpinski Triangle Visualizer
//Assignment 5 COP 3330

//Import all necessary things
import java.awt.BorderLayout;
import java.awt.Canvas;
import java.awt.Color;
import java.awt.Dimension;
import java.awt.FlowLayout;
import java.awt.Graphics;
import java.awt.GridLayout;
import java.awt.Polygon;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.ItemListener;

import javax.swing.AbstractButton;
import javax.swing.Box;
import javax.swing.BoxLayout;
import javax.swing.JButton;
import javax.swing.JCheckBox;
import javax.swing.JComboBox;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JOptionPane;
import javax.swing.JPanel;
import javax.swing.JTextField;
import javax.swing.SpringLayout;


public class SierpinskiVisualizer extends JFrame{

	private static final int SIZE = 512; // should be a power of 2
	
	// Colors to display.
	private static enum COLOR {
		BLUE(Color.BLUE,"Blue"),
		BLACK(Color.BLACK, "Black"),
		RED(Color.RED, "Red"),
		CYAN(Color.CYAN, "Cyan"),
		DARKGRAY(Color.DARK_GRAY, "Dark Gray"), 
		ORANGE(Color.ORANGE, "Orange"),
		MAGENTA(Color.MAGENTA, "Magenta"),
		GREEN(Color.GREEN, "Green"),
		PINK(Color.PINK, "Pink"),
		WHITE(Color.WHITE, "White"),
		YELLOW(Color.YELLOW,"Yellow");
		
			
		
		
		private Color color;
		private String name;
		
		COLOR(Color c, String n) {
			this.color = c;
			this.name = n;
		}
		
		private Color getColor() {
			return this.color;
		}
		
		public String toString() {
			return this.name;
		}
	}
	
	// GUI components
	private JFrame frame;
	private Canvas canvas;
	private Graphics graphics;
	private JTextField recDepthTextField;
	private JComboBox[] colorComboBox;
	private JCheckBox randomizeCheckBox;
	private JButton button;
	private boolean selected;
	
	
	
	// Used to keep track of coloring the triangles correctly.
	private Color[] colorScheme;
	private int depth;
	
	// Generate the GUI
	public SierpinskiVisualizer() {
		
		// Build the frame.
		JFrame frame = new JFrame("Sierpinski Visualizer");
		frame.setSize(1000, 600);
		
		// Create the canvas.
		canvas = new Canvas();
		canvas.setSize(SIZE, SIZE);
		canvas.setBackground(Color.BLACK);
		
		// Group all the user input together.
		JPanel main = new JPanel();
		JPanel[] main2 = new JPanel[10];
		FlowLayout layout = new FlowLayout();
		frame.setLayout(layout);
		main.setLayout(new BoxLayout(main, BoxLayout.Y_AXIS));
		for(int i=0; i<9; ++i){
			main2[i] = new JPanel();
			main2[i].setLayout(layout);
		}
		
		// Add recursive depth input.
		JLabel text = new JLabel("Recursive Depth:");
		recDepthTextField = new JTextField("0", 7);
		main2[1].add(text);
		main2[1].add(recDepthTextField);
	
		// Add color selection input.
		String[] boxcolors = {"CYAN", "RED", "BLUE", "YELLOW", "GREEN"};
		String[] names = {"Color 1", "Color 2", "Color 3", "Color 4", "Color 5"};
		colorComboBox = new JComboBox[5];
		for(int i=0; i<5; ++i){
			JLabel name = new JLabel("Color "+(i+1));
			colorComboBox[i] = new JComboBox(boxcolors);
			main2[i+2].add(name);
			main2[i+2].add(colorComboBox[i]);
		}
		
		// Add randomize input. Put a listener on the check box to control whether the colors are
		//	enabled.
		JCheckBox randomizeCheckBox = new JCheckBox("Randomize colors at each level");
		randomizeCheckBox.addActionListener(new ActionListener() { 
			public void actionPerformed(ActionEvent actionEvent){
				AbstractButton abstractButton = (AbstractButton) actionEvent.getSource();
				selected = abstractButton.getModel().isSelected();
				if(selected == true){
					for(int i=0; i<5; ++i)
						colorComboBox[i].setEnabled(false);
				}
				else{
					for(int i=0; i<5; ++i)
						colorComboBox[i].setEnabled(true);
				}
			}
		});
		
		// Draw button and add listener. 
		button = new JButton("Draw!");
		button.addActionListener(new ActionListener() {
			public void actionPerformed(ActionEvent e){
				draw();
			}
		});
		
		
		// Put the user input on the frame
		main2[7].add(randomizeCheckBox);
		main2[8].add(button);
		for(int i=1; i<9; ++i)
			main.add(main2[i]);
		frame.add(canvas);
		frame.add(main);
		frame.setVisible(true);
		frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		
		// Get the graphics object of the canvas. It's important to do this AFTER the frame is
		//	visible, since before this there is no graphics object associated with the canvas.
		graphics = canvas.getGraphics();
	}
	
	// Draw it!
	private void draw() {
		
		// Erase the old image.
		graphics.clearRect(0, 0, SIZE, SIZE);
		
		// Try and get the depth
		String d = recDepthTextField.getText();
		depth = Integer.parseInt(d);
		try {
			//check if depth is in correct format/range else throw NumberFormatException();
			if(depth<1 || depth>10)
				throw new NumberFormatException();
			
		} catch (NumberFormatException ex) {

			//show some error message
			JOptionPane.showMessageDialog(frame, "Error Depth not between ranges (1,10)");
			return;
		}
		
		// Generate the color scheme to use.
		colorScheme = new Color[5];
		//Random Scheme
		if (selected == true){
			colorScheme[0] = Color.RED;
			colorScheme[1] = Color.CYAN;
			colorScheme[2] = Color.MAGENTA;
			colorScheme[3] = Color.YELLOW;
			colorScheme[4] = Color.GREEN;
		}
		else {
			for(int i=0; i<5; i++){
				String c = colorComboBox[i].getSelectedItem().toString();
				COLOR e = COLOR.valueOf(c);
				colorScheme[i] = e.getColor();
			}
		}
		
		// Draw the base triangle.
		graphics.drawLine(0, SIZE, SIZE/2, 0);
		graphics.drawLine(SIZE/2, 0, SIZE, SIZE);
		
		
		// Now draw the rest of the inner triangles with the recursive function.
		draw(depth,0,0,SIZE);
	}
	
	// Recursive function to draw triangles at a given depth at the specified square given.
	private void draw(int d, int x, int y, int S) {
		if (d==0)
			return;
		
		// Otherwise, draw big triangle at this level, between the points
		// shown in the figure. You can use the fillPolygon() method of
		// the Graphics object of your Canvas. Make sure you get the color
		// right!
		
		//Get color to fill
		int newdepth;
		if(d>=5)
			newdepth = (d-5);
		else
			newdepth = d;
		Color c = colorScheme[newdepth];
		graphics.setColor(c);
		
		
		//Create Triangle and fill it in with color
		Polygon p = new Polygon();
		p.addPoint(x+S/4, y+S/2);
		p.addPoint(x+3*S/4, y+S/2);
		p.addPoint(x+S/2, y+S);
		graphics.fillPolygon(p);
		
		
		
		
		// Draw the subtriangles. The self-similarity of fractals means
		// that they are themselves Sierpinski triangles of depth d-1.
		// draw top middle triangle
		draw(d-1, x+S/4, y, S/2);
		
		// draw bottom left triangle
		draw(d-1, x, y+S/2, S/2);
		
		// draw bottom right triangle
		draw(d-1, x+S/2, y+S/2, S/2);
	}
	
	public static void main(String[] args) {
		new SierpinskiVisualizer();
	}

}
