
import java.applet.*;
import java.awt.*;
import java.net.*;
import java.awt.event.*;
import javax.swing.*;


public class BallApplet2 extends Applet implements Runnable {
		int x_pos = 10;
		int y_pos = 100;
		int delay = 10;
		private Image dbImage;//db stands for "double buffer"
		private Graphics dbg;
		AudioClip bounce;
		Image background;
		Robot robot;
		Color color;
		Font font;
		private Point p1;

	public void init() 
	{
		Mousey mousey = new Mousey();
		addMouseListener (mousey);
		addMouseMotionListener(mousey);
		KeyMe keyme = new KeyMe();
		addKeyListener(keyme);
		setSize(900,720);
		bounce = getAudioClip (getCodeBase(), "bounce.au");
		background = getImage (getCodeBase (), "background.jpg"); 
		nelson = getImage(getCodeBase(), "nelson.gif");
		font = new Font("Times New Roman", Font.BOLD, 20);
 	}
	
public class KeyMe implements KeyListener
{
	public void keyPressed(KeyEvent event)
	{
		int key = event.getKeyCode();
		
		if (key == KeyEvent.VK_LEFT)
		{ 
			x_pos -=30;
			bounce.play();
		}
		else if (key == KeyEvent.VK_RIGHT)
		{ 
			x_pos +=30;
			bounce.play();
		}
		else if (key == KeyEvent.VK_UP)
		{ 
			y_pos -=30;
			bounce.play();
		}
	 	else if (key == KeyEvent.VK_DOWN)
		{ 
			y_pos +=30;
			bounce.play();
		}
		else if (key == 32)
		{ 
			x_pos -=30;
			bounce.play();
		}
		else
		{ 
			System.out.println ("Charakter: " + (char)key + " Integer Value: " + key);
		}
	}//end KeyPressed
	
	public void keyTyped(KeyEvent event){}
	public void keyReleased(KeyEvent event){}
}//end KeyMe class



private class Mousey implements MouseListener, MouseMotionListener
{
	public void mouseClicked (MouseEvent e) 
	{ 
		p1 = e.getPoint();
		int x = (int) p1.getX();
		int y = (int) p1.getY();
		x_pos -=30; 
	} 
	
	public void mousePressed(MouseEvent e){}
	public void mouseReleased(MouseEvent e){}
	public void mouseEntered(MouseEvent e){}
	public void mouseExited(MouseEvent e){}
	public void mouseMoved(MouseEvent e){}
	public void mouseDragged(MouseEvent e){}
}//end Mousey class

	public void start () //works in sync with the run method
	{ 
		Thread th = new Thread (this);
		th.start ();
	} 
	
	public void run () 
	{ 
	  	Thread.currentThread().setPriority(Thread.MIN_PRIORITY);
		
		while (true)
		{
			x_pos ++;//this is what makes the ball move on a timer thread
			repaint();//this calls the paint method.  The screen is continually repainted as time goes by

				try
				{ 	
				Thread.sleep (delay);//you can change delay for higher or lower speeds.
				}
				catch (InterruptedException ex)
				{ }
				
			Thread.currentThread().setPriority(Thread.MAX_PRIORITY);
		} //end while
	}// end run


 	public void paint (Graphics g)//this is what actually gets painted
	{	g.drawImage (background, 0, 0, this);
		g.setColor (Color.red);
		g.fillOval (x_pos , y_pos , 20, 20);
		g.setColor(Color.blue);
		g.fillRect(300, 75, 40, 40);
		g.setFont(font);
		g.drawString("Write String here!", 100,300);
	} 


//Don't touch this method, it will double buffer your applet making it appear smoother
	public void update (Graphics g)
	{ 
		if (dbImage == null)
		{ 
			dbImage = createImage (this.getSize().width, this.getSize().height);
			dbg = dbImage.getGraphics ();
		}
			dbg.setColor (getBackground ());
			dbg.fillRect (0, 0, this.getSize().width, this.getSize().height);
			dbg.setColor (getForeground());
			paint (dbg);
			g.drawImage (dbImage, 0, 0, this);
		}//end update method
		
}//end BallApplet
