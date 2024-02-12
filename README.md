package Projekat;

import java.awt.BorderLayout;
import java.awt.Color;
import java.awt.Font;
import java.awt.GridLayout;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.nio.file.FileAlreadyExistsException;
import java.nio.file.Files;
import java.nio.file.Paths;

import javax.swing.BorderFactory;
import javax.swing.Box;
import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JPanel;
import javax.swing.JTextArea;

import static javax.swing.JOptionPane.showMessageDialog;

public class Body implements ActionListener {
	
	private String[] latin = {" ", "a", "b", "v", "g", "d", "đ", "e", "ž", "z", "i", "j", "k", "l", "lj", "m", "n",
			"nj", "o", "p", "r", "s", "t", "ć", "u", "f", "h", "c", "č", "dž", "š", "A", "B", "V", "G", "D", 
			"Đ", "E", "Ž", "Z", "I", "J", "K", "L", "Lj", "M", "N", "Nj", "O", "P", "R", "S", "T", "Ć", "U", "F",
			"H", "C", "Č", "Dž", "Š", "!", "?", ":"};
	private char[] cyrillic = {' ', 'а', 'б', 'в', 'г', 'д', 'ђ', 'е', 'ж', 'з', 'и', 'ј', 'к', 'л', 'љ', 'м', 'н', 
			'њ', 'о', 'п', 'р', 'с', 'т', 'ћ', 'у', 'ф', 'х', 'ц', 'ч', 'џ', 'ш', 'А', 'Б', 'В', 'Г', 'Д', 'Ђ', 
			'Е', 'Ж', 'З', 'И', 'Ј', 'К', 'Л', 'Љ', 'М', 'Н', 'Њ', 'О', 'П', 'Р', 'С', 'Т', 'Ћ', 'У', 'Ф', 'Х', 
			'Ц', 'Ч', 'Џ', 'Ш', '!', '?', ':'};
	
	private JFrame frame;
	private JPanel panel;
	private JLabel label;
	private JLabel labelTwo;
	private JTextArea textArea;
	private JTextArea textAreaOutput;
	private JButton button;
	private JButton convert;
	private JButton refresh;
	
	private String input;
	private String result;
	private String path;
	
	public Body() {
		
		label = new JLabel("ENTER");
		label.setFont(new Font("Serif", Font.BOLD, 24));

		labelTwo = new JLabel("OUTPUT");
		labelTwo.setFont(new Font("Serif", Font.BOLD, 24));
		
		textArea = new JTextArea(5, 20);
		textArea.setBorder(BorderFactory.createEmptyBorder(5, 5, 5, 5));
		textArea.setLineWrap(true);
		textArea.setWrapStyleWord(true);

		textAreaOutput = new JTextArea();
		textAreaOutput.setBorder(BorderFactory.createEmptyBorder(5, 5, 5, 5));
		textAreaOutput.setLineWrap(true);
		textAreaOutput.setWrapStyleWord(true);
		
		button = new JButton("CONVERT");
		button.addActionListener(this);
		button.setForeground(Color.blue);

		convert = new JButton("REFRESH");
		convert.addActionListener(this);
		convert.setForeground(Color.red);

		refresh = new JButton("SAVE TEXT");
		refresh.addActionListener(this);
		refresh.setForeground(Color.green);
		
		panel = new JPanel();
		panel.setBorder(BorderFactory.createEmptyBorder(30, 30, 30, 30));
		panel.setLayout(new GridLayout(3, 8));
		panel.add(label);
		panel.add(textArea);
		panel.add(button);
		panel.add(labelTwo);
		panel.add(textAreaOutput);
		panel.add(refresh);
		panel.add(Box.createHorizontalStrut(1));
		panel.add(convert);
		
		frame = new JFrame();
		frame.add(panel, BorderLayout.CENTER);
		frame.pack();
		frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		frame.setVisible(true);
		frame.setTitle("Script converter: Luka Gaća MIT8/23");
	}
	
	public static void main(String[] args) {
		new Body();
	}
	@Override
	public void actionPerformed(ActionEvent e) {
		try {
			if(e.getSource() == button) {
				input = textArea.getText();
				if(textArea.getText() == "") {
					showMessageDialog(frame, "Prazno text polje!");
				}
				else {
					StringBuilder builder = new StringBuilder();
					for (int i = 0; i < input.length(); i++) {
						for (int j = 0; j < cyrillic.length; j++ ) {
							if (input.charAt(i) == cyrillic[j]) {
								builder.append(latin[j]);
							}
						}
					}
			    textAreaOutput.append(builder.toString());
			    }
			}
			else if(e.getSource() == convert) {
				textArea.setText("");
				textAreaOutput.setText("");
			}
			else if(e.getSource() == refresh) {
				try {
					result = textAreaOutput.getText();
					path = "Output.txt";
					Files.write(Paths.get(path), result.getBytes());
				} 
				catch (FileAlreadyExistsException fileExists) {
					showMessageDialog(frame, "Hej, pazi na " + fileExists);
				}
			}
		} 
		catch (Exception error) {
			button.removeActionListener(this);
			convert.removeActionListener(this);
			showMessageDialog(frame, "Hej, pazi na " + error);
		}
	}
}
