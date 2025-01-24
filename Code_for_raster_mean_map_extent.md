with_variable('Raster_Layer', 'Heisse_Tage_1991_2020',
with_variable('cell_width', 1000,
with_variable('cell_height', 1000,
with_variable('extent', @map_extent,

    array_mean(
        array_foreach(
            generate_series(
                ceil(x_min(@extent) / @cell_width) * @cell_width,  -- First aligned X coordinate
                floor(x_max(@extent) / @cell_width) * @cell_width,  -- Last aligned X coordinate
                @cell_width
            ),
            with_variable('x_pos', @element,
                array_mean(  -- This combines all Y positions for each X position
                    array_foreach(
                        generate_series(
                            ceil(y_min(@extent) / @cell_height) * @cell_height,  -- First aligned Y coordinate
                            floor(y_max(@extent) / @cell_height) * @cell_height,  -- Last aligned Y coordinate
                            @cell_height
                        ),
                        with_variable('y_pos', @element,
                            raster_value(  -- Retrieve raster value for each X, Y point
                                @Raster_Layer,
                                1,  -- Assuming first band
                                make_point(@x_pos, @y_pos)
                            )
                        )
                    )
                )
            )
        )
    )

))))