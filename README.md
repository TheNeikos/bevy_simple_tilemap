# bevy_simple_tilemap

Refreshingly simple tilemap implementation for Bevy Engine.

## Why another tilemap?

The main reason I started this was because I felt the existing tilemap implementations for Bevy were needlessly complicated to use when all you want to do is to as quickly and simply as possible render a grid of tiles to the screen, often exposing internal implementation details such as chunks to the user.

## Goals:
* Allow the user to render a grid of rectangular tiles to the screen
* Make this as simple and intuitive as possible

## Non-goals:
* Supporting every imaginable shape of tile
* 3D tilemaps
* Assisting with non-rendering-related game-logic

## How to use:

### Spawning:
```rust
fn setup(
  asset_server: Res<AssetServer>,
  mut commands: Commands,
  mut texture_atlases: ResMut<Assets<TextureAtlas>>,
) {
    // Load tilesheet texture and make a texture atlas from it
    let texture_handle = asset_server.load("textures/tilesheet.png");
    let texture_atlas = TextureAtlas::from_grid(texture_handle, Vec2::new(16.0, 16.0), 4, 1);
    let texture_atlas_handle = texture_atlases.add(texture_atlas);

    // Set up tilemap
    let tilemap_bundle = TileMapBundle {
        texture_atlas: texture_atlas_handle.clone(),
        ..Default::default()
    };

    // Spawn tilemap
    commands.spawn_bundle(tilemap_bundle);
}
```

### Updating (or inserting) single tile:
```rust
tilemap.set_tile(IVec3::new(0, 0, 0), Some(Tile { sprite_index: 0, color: Color::WHITE }));
```

### Updating (or inserting) multiple tiles:
```rust
// List to store set tile operations
let mut tiles: Vec<(IVec3, Option<Tile>)> = Vec::new();
tiles.push((IVec3::new(0, 0, 0), Some(Tile { sprite_index: 0, color: Color::WHITE })));
tiles.push((IVec3::new(1, 0, 0), Some(Tile { sprite_index: 1, color: Color::WHITE })));

// Perform tile update
tilemap.set_tiles(tiles);
```
