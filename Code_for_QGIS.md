# QGIS Expression builder

## Minimal Circle
	make_circle(
		centroid(@geometry),
		distance(
			centroid(@geometry),
			closest_point(boundary(@geometry), centroid(@geometry))
		)
	)
	
## Simplify & Smooth
	smooth(
		Simplify(
		@geometry,
		5 -- increase this value to Simplify
		),
		2 -- increase this value to smooth
	
	)

## Join by Location in Expression builder
	overlay_within( --other spatial relationsships are also available
		'layer'
		,filter:= "size" > 10 --filter the comparison layer before comparing
		,expression:= "field_name" --if no expression is given it will just test wether the feature is within the comparing layer
	)
	
## Equal Interval Buffer
	-- creates buffer at 5000, 10000, 15000, 20000
	collect_geometries(
		array_foreach(
			generate_series(1,4),
			buffer(
				@geometry,
				@element * 5000 -- Buffer value
			)
		)
	)

## Exponential Interval Buffer
	-- creates buffer at 5000, 10000, 20000
	collect_geometries(
		array_foreach(
			generate_series(0,3),
			buffer(
				@geometry,
				(2^@element) * 5000 -- Buffer value
			)
		)
	)
	
## Beveled Label callouts
	with_variable(
		'start',
		make_point("auxiliary_storage_labeling_positionx", "auxiliary_storage_labeling_positiony"),
	with_variable(
		'end',
		centroid(@geometry),
	with_variable(
		'bevel',
		50,  -- Adjust bevel position as needed (higher values push it closer to start/ end position)
	with_variable(
		'bevel_start',
		make_point(x(@start), y(@end) + @bevel),
	with_variable(
		'bevel_end',
		make_point(x(@start) - @bevel, y(@end)),
	
	-- line creation
	make_line(@start, @bevel_start, @bevel_end, @end)

	-- end of with_varibale
	)))))
	
## Draw line from point to closest point on line
	make_line(
		closest_point(
			geom_from_wkt(
				array_to_string(
					overlay_nearest(
						'line_layer',
						expression:=geom_to_wkt($geometry)
					)
				)
			),
			$geometry
		),
		$geometry
	)
