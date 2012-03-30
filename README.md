JFastEMD
========

This is a pure Java implementation of the Fast Earth Mover's Algorithm by Ofir Pele and Michael Werman. In fact, it's a partial port of the C++ implementation that can be found here:

http://www.cs.huji.ac.il/~ofirpele/FastEMD/code/

Install
-------

JFastEMD is meant to be easily incorporated into your project by copying the following files:

* `JFastEMD.java`
* `Signature.java`
* `Feature.java`
* `Aux.java`

How to use
----------

The main entry point is the static method JFastEMD.distance with the following signature:

    double distance(Signature signature1, Signature signature2, double extraMassPenalty)
    
* `signature1` and `signature2`: the sparse matrices being compared - more details ahead
* `extraMassPenalty`: penalty for extra mass. 0 for no penalty, -1 for the default, other positive values to specify the penalty

An `extraMassPenalty` of -1 means that the extra mass penalty is the maximum distance found between two features.

Signatures can be used to represent sparse n-dimensional matrices. They are a collection of features and their respective weights. `Feature` is an interface that you must implement for your specific application.

`Test.java` and `Feature2D.java` provide a usage example, where we compute EMD distances between two-dimensional matrices using Cartesian distances.

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

The two-dimensional matrices can be converted to signature like so:

	static Signature getSignature(double[] map, int bins) {

		// find number of entries in the sparse matrix
        int n = 0;
        for (int x = 0; x < bins; x++) {
            for (int y = 0; y < bins; y++) {
                if (getValue(map, x, y, bins) > 0) {
                    n++;
                }
            }
    	}
        
    	// compute features and weights
    	Feature2D[] features = new Feature2D[n];
        double[] weights = new double[n];
        int i = 0;
        for (int x = 0; x < bins; x++) {
            for (int y = 0; y < bins; y++) {
                double val = getValue(map, x, y, bins);
                if (val > 0) {
                    Feature2D f = new Feature2D(x, y);
                    features[i] = f;
                    weights[i] = val;
                    i++;
                }
            }
        }

        Signature signature = new Signature();
        signature.setNumberOfFeatures(n);
        signature.setFeatures(features);
        signature.setWeights(weights);

        return signature;
    }