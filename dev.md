# Element 5
In Element 5, we have included the following:
* Arc Rotate Camera
* Imported Mesh
* Player Movement
* Animation
* GUI that transitions to a main scene
* Movement in the form of a moveable character
* physics in the characters football

# Scripts made for element 5
### Goalpoasts
```typescript
function Goalpoats(scene: Scene) {
  const spriteManagerGoals = new SpriteManager("GoalManager", "textures/Goalpost_PNG.png", 2000, {width: 5261, height: 3140}, scene);
  const goal = new Sprite("GOAL", spriteManagerGoals); 
  goal.position.x = 0;
  goal.position.z = 4;
  goal.position.y = 1;
  goal.width = 4;
  goal.height = 3;
  return spriteManagerGoals;
  }
```
this script creates the goalposts, as a sprite specifically because i just liked the idea

### Initialising Havok physics engine
```typescript
  let initializedHavok;
  HavokPhysics().then((havok) => {
    initializedHavok = havok;
  });

  const havokInstance = await HavokPhysics();
  const havokPlugin = new HavokPlugin(true, havokInstance);

  globalThis.HK = await HavokPhysics();
```
this allows me to add physics to the objects that need it

### Create the football
```typescript
  function createSphere(scene: Scene, x: number, y: number, z: number) {
    let sphere = MeshBuilder.CreateSphere("sphere",{ diameter: 2, segments: 32 },scene,)
    const ballMat = new StandardMaterial("ballMat",scene)
    ballMat.diffuseTexture = new Texture("textures/FootballTexture.jpg")
    sphere.position.x = x;
    sphere.position.y = y;
    sphere.position.z = z;
    sphere.material = ballMat;
    const shpereAggregate = new PhysicsAggregate(sphere, PhysicsShapeType.SPHERE,{mass: 1},scene);
    
    return sphere;
  }
```
this makes the football, textures it and then adds physics to it so it can be moved around

