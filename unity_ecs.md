### IComponentData example

``` cs
using Unity.Entities;
using Unity.Transforms;

// this line will allow attach script in the editor
[GenerateAuthoringComponent]
public struct PlanetData : IComponentData
{
    public Translation target;
    public float speed;
}

```


### JobComponentSystem example

``` cs
  using Unity.Entities;
  using Unity.Mathematics;
  using Unity.Transforms;
  using Unity.Jobs;
  
  public class MoveSystem : JobComponentSystem
  {
      protected override JobHandle OnUpdate(JobHandle inputDeps)
      {
          var jobHandle = Entities
                 .WithName("MoveSystem")
                 .ForEach((ref Translation position, ref Rotation rot) =>
                 {
                      // Do something there
                 })
                 .Schedule(inputDeps);

          return jobHandle;
      }

  }
```

### Object construction from prefab

``` cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using Unity.Entities;
using Unity.Mathematics;
using Unity.Transforms;


public class EcsManager : MonoBehaviour
{
    private EntityManager _manager;
    [SerializeField]
    private GameObject _sheepPrefab;
    private const int numSheep = 1000;
    
    void Start()
    {
        _manager =  World.DefaultGameObjectInjectionWorld.EntityManager;
        var settings = GameObjectConversionSettings.FromWorld(World.DefaultGameObjectInjectionWorld, null);
        var prefab = GameObjectConversionUtility.ConvertGameObjectHierarchy(_sheepPrefab, settings);
        for (int i = 0; i < numSheep; i++)
        {
            var instance = _manager.Instantiate(prefab);
            var pos = transform.TransformPoint(new float3(UnityEngine.Random.Range(-50, 50),UnityEngine.Random.Range(0, 100),UnityEngine.Random.Range(-50, 50)));
            _manager.SetComponentData(instance, new Translation { Value = pos });
            _manager.SetComponentData(instance, new Rotation { Value = new quaternion(0,0,0,0)});
        }
        
    }
}
```

### Object construction from code

``` cs
using System.Collections;
using System.Collections.Generic;
using Unity.Entities;
using Unity.Mathematics;
using Unity.Rendering;
using Unity.Transforms;
using UnityEngine;
using Random = UnityEngine.Random;

public class ECSManager : MonoBehaviour
{
    private EntityManager _manager;
    public Mesh mesh;
    public Material material;
    public GameObject sun;
    private const int numPlanets = 10000;
    
    private void Start()
    {
        _manager = World.DefaultGameObjectInjectionWorld.EntityManager;
        var arch = _manager.CreateArchetype(
            typeof(PlanetData), 
            typeof(Translation), 
            typeof(Rotation), 
            typeof(RenderMesh), 
            typeof(LocalToWorld),
            typeof(NonUniformScale));

        for (int i = 0; i < numPlanets; i++)
        {
            var instance = _manager.CreateEntity();
            var x = Mathf.Sin(i) * Random.Range(35, 100);
            var z =  Mathf.Cos(i) * Random.Range(35, 100);
            var y = Random.Range(-2, 2);
            var pos = new float3(x,y,z);
            _manager.SetArchetype(instance, arch);
            
            var size = Random.Range(0.3f, 2f);
            
            _manager.SetComponentData(instance, new PlanetData
            {
                target = new Translation
                {
                    Value = sun.transform.position
                },
                speed = 2.3f - size
            });
            
            _manager.SetComponentData(instance, new NonUniformScale
            {
                Value = new float3(size,size,size)
            });
            
            _manager.SetComponentData(instance, new Translation { Value = pos });
            _manager.SetComponentData(instance, new Rotation { Value = quaternion.identity });
            _manager.SetSharedComponentData(instance, new RenderMesh
            {
                mesh = mesh,
                material = material,
            });
        }
    }
    
}
```

### Orbit around pivot

``` cs
            float3 pivot = planetData.target.Value;
            position.Value = math.mul(quaternion.AxisAngle(new float3(0, 1, 0), deltaTime * planetData.speed), 
                  position.Value - pivot) + pivot;
```
