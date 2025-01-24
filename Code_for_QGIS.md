# QGIS Expression builder

# Minimal Circle
	make_circle(
		centroid(@geometry),
		distance(
			centroid(@geometry),
			closest_point(boundary(@geometry), centroid(@geometry))
		)
	)
	
# Simplify & Smooth
	smooth(
		Simplify(
		@geometry,
		5 -- increase this value to Simplify
		),
		2 -- increase this value to smooth
	
	)
