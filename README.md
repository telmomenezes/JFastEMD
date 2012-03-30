JFastEMD
========

This is a pure Java implementation of the Fast Earth Mover's Algorithm by Ofir Pele and Michael Werman. In fact, it's a partial port of the C++ implementation that can be found here:

http://www.cs.huji.ac.il/~ofirpele/FastEMD/code/

= How to use

The main entry point is the static method JFastEMD.distance with the following signature:

    double distance(Signature signature1, Signature signature2, double extraMassPenalty)
    
* signature1 and signature2 represent the sparse matrices being compared - more details ahead
* extraMassPenalty penalty for extra mass. 0 for no penalty, -1 for the default, other positive values to specify the penalty

An extraMassPenalty of -1 means that the extra mass penalty is the maximum distance found between two features.

Signatures can be used to represent sparse n-dimensional matrices. They are a collection of features and their respective weights. `Feature is an interface that you must implement for your specific application.

Please refer to `Test.java and `Feature2D.java for a usage example, where we compute EMD distances between two-dimensional matrices using Cartesian distances.

	public class Feature2D implements Feature {
    	private double x;
    	private double y;

    	public Feature2D(double x, double y) {
        	this.x = x;
        	this.y = y;
    	}
    
    	public double groundDist(Feature f) {
        	Feature2D f2d = (Feature2D)f;
        	double deltaX = x - f2d.x;
        	double deltaY = y - f2d.y;
        	return Math.sqrt((deltaX * deltaX) + (deltaY * deltaY));
    	}
	}