# ConnectedComponent-package imageprocessing;

import java.util.ArrayList;
import java.awt.Color;

import edu.princeton.cs.introcs.Picture;

/*************************************************************************
 * Compilation: java ConnectedComponentImage.java
 * 
 * The <tt>ConnectedComponentImage</tt> class
 * <p>
 * You do the rest....
 * 
 * @author
 *************************************************************************/
public class ConnectedComponentImage {

	Picture picture;
	private int count;
	private int[] parent; // the parent of the subtrees
	private int[] size; // for the # of subtree componments

	/**
	 * Initialise fields
	 * 
	 * @param fileLocation
	 */
	public ConnectedComponentImage(String fileLocation) {
		picture = getPicture(fileLocation);
		binaryComponentImage();
		
		
		WeightedQuickUnionUF(picture.width()*picture.height());
        for (int i = 0; i < parent.length; i++) {
            int p = i;
            i++;
            int q = i;
            if (connected(p, q)){
            	union(p, q);
            	System.out.println(p + " " + q);            	
            }
        }
	}

	public static void main(String args[]) {
		ConnectedComponentImage app = new ConnectedComponentImage("C:/Users/Adam/Downloads/Assign1Starter/ConnectorStarter/images/shapes.bmp");
	}

	/**
	 * Returns the number of components identified in the image.
	 * 
	 * @return the number of components (between 1 and N)
	 */
	public int countComponents() {
		// TODO
		return count;
	}

	/**
	 * Returns the original image with each object bounded by a red box.
	 * 
	 * @return a picture object with all components surrounded by a red box
	 */

	public Picture identifyComonentImage() {

		Picture pic = getPicture();

		int maxX = 0;
		int minX = pic.width();
		int maxY = 0;
		int minY = pic.height();

		for (int x = 0; x < pic.width(); x++) {
			for (int y = 0; y < pic.height(); y++) {
				if (!pic.get(x, y).equals(Color.WHITE)) {

					if (x < minX)
						minX = x;
					if (x > maxX)
						maxX = x;
					if (y < minY)
						minY = y;
					if (y > maxY)
						maxY = y;

				}
			}

		}

		if (minX > maxX || minY > maxY) {
			System.out.println("wooow thats very white");
		} else {
			for (int x = minX; x <= maxX; x++) {
				pic.set(x, minY, Color.GREEN);
				pic.set(x, maxY, Color.GREEN);
			}

			for (int y = minY; y <= maxY; y++) {
				pic.set(minX, y, Color.BLUE);
				pic.set(maxX, y, Color.BLUE);
			}
		}
		return picture;
	}

	/**
	 * Returns a picture with each object updated to a random colour.
	 * 
	 * @return a picture object with all components coloured.
	 */
	public Picture colourComponentImage() {

		return null;

	}

	public Picture getPicture(String fileLocation) {
		picture = new Picture(fileLocation);
		return picture;
		
		//return new Picture("fileLocation");
	}

	/**
	 * Returns a binarised version of the original image
	 * 
	 * @return a picture object with all components surrounded by a red box
	 */
	public Picture binaryComponentImage() {
		//Picture pic = new Picture("C:/Users/Adam/Downloads/Assign1Starter/ConnectorStarter/images/shapes.bmp");
		int width = picture.width();
		int height = picture.height();
		double thresholdPixelValue = 128.0; // 0 = black, 255 = white
		// convert to black/white;
		for (int x = 0; x < width; x++) {
			for (int y = 0; y < height; y++) {
				Color c = picture.get(x, y);
				if (Luminance.lum(c) < thresholdPixelValue) {
					picture.set(x, y, Color.BLACK);
				} else {
					picture.set(x, y, Color.WHITE);
				}
			}
		}
		picture.show();
		return picture;
	}

	// public void weightedQU(){
	public void WeightedQuickUnionUF(int N) {
		count = N;
		parent = new int[N];
		size = new int[N];
		for (int i = 0; i < N; i++) {
			parent[i] = i;
			size[i] = 1;
		}
	}

	public int find(int p) {
		validate(p);
		while (p != parent[p])
			p = parent[p];
		return p;
	}

	private void validate(int p) {
		int N = parent.length;
		if (p < 0 || p >= N) {
			throw new IndexOutOfBoundsException("index " + p
					+ " is not between 0 and " + (N - 1));
		}
	}

	public boolean connected(int p, int q) {
		return find(p) == find(q);
	}

	public void union(int p, int q) {
		int rootP = find(p);
		int rootQ = find(q);
		if (rootP == rootQ)
			return;

		// make smaller root point to larger one
		if (size[rootP] < size[rootQ]) {
			parent[rootP] = rootQ;
			size[rootQ] += size[rootP];
		} else {
			parent[rootQ] = rootP;
			size[rootP] += size[rootQ];
		}
		count--;
	}

}
