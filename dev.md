# Element 5
In Element 5, we have included the following:
* Arc Rotate Camera
* Imported Mesh
* Player Movement
* Animation
* GUI that transitions to a main scene
* Light and shadows from the football spotlight
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
```typescript
that.goal = Goalpoats(that.scene);
```
this script creates the goalposts, as a sprite specifically because i just liked the idea

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
```typescript
that.sphere = createSphere(that.scene, 0, 2, 0);
```
this makes the football, textures it and then adds physics to it so it can be moved around

### Create the spotlight for the football
```typescript
function createAnyLight(scene: Scene, index: number, px: number, py: number, pz: number, colX: number, colY: number, colZ: number, mesh: Mesh) {
    // only spotlight, point and directional can cast shadows in BabylonJS
    switch (index) {
      case 1: //hemispheric light
        const hemiLight = new HemisphericLight("hemiLight", new Vector3(px, py, pz), scene);
        hemiLight.intensity = 0.1;
        return hemiLight;
        break;
      case 2: //spot light
        const spotLight = new SpotLight("spotLight", new Vector3(px, py, pz), new Vector3(0, -1, 0), Math.PI / 3, 10, scene);
        spotLight.diffuse = new Color3(colX, colY, colZ); //0.39, 0.44, 0.91
        let shadowGenerator = new ShadowGenerator(1024, spotLight);
        shadowGenerator.addShadowCaster(mesh);
        
        shadowGenerator.useExponentialShadowMap = true;
        return spotLight;
        break;
      case 3: //point light
        const pointLight = new PointLight("pointLight", new Vector3(px, py, pz), scene);
        pointLight.diffuse = new Color3(colX, colY, colZ); //0.39, 0.44, 0.91
        shadowGenerator = new ShadowGenerator(1024, pointLight);
        shadowGenerator.addShadowCaster(mesh);
        shadowGenerator.useExponentialShadowMap = true;
        return pointLight;
        break;
    }
  }
```
```typescript
that.light = createAnyLight(that.scene, 2, 0, 5, 0, 0.75, 0.12, 0.91, that.sphere)
```