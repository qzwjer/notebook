
set_cell

void set_cell(layer: int, coords: Vector2i, source_id: int = -1, atlas_coords: Vector2i = Vector2i(-1, -1), alternative_tile: int = 0)

Sets the tile identifiers for the cell on layer layer at coordinates coords. Each tile of the TileSet is identified using three parts:

- The source identifier source_id identifies a TileSetSource identifier. See TileSet.set_source_id(),

- The atlas coordinates identifier atlas_coords identifies a tile coordinates in the atlas (if the source is a TileSetAtlasSource). For TileSetScenesCollectionSource it should always be Vector2i(0, 0)),

- The alternative tile identifier alternative_tile identifies a tile alternative in the atlas (if the source is a TileSetAtlasSource), and the scene for a TileSetScenesCollectionSource.

If source_id is set to -1, atlas_coords to Vector2i(-1, -1) or alternative_tile to -1, the cell will be erased. An erased cell gets all its identifiers automatically set to their respective invalid values, namely -1, Vector2i(-1, -1) and -1.

If layer is negative, the layers are accessed from the last one.

以上是官方文档中set_cell函数的说明，注意第二个参数coords不是全局或局部坐标，而是tilemap的位置，例如Vector2i(1,1)可能代表tilemap坐标（1,1）的大小X像素的格子，X为设置的tile_size的大小


1. 使用噪声绘制地图
   缺点：地图粗糙，没有规划

2. 