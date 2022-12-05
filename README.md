# Coding-One
# Final Project Description 


## Geometrical Band - Practice Version
- https://youtu.be/V2ieqrnRGjw

I use Three.js to build the geometry and skybox, add Positional Audio to the scene and bind to the different geometry to make them sound like different instruments. I then used the TrackballControls plugin to get the interactive functionality I wanted, i.e. left mouse button to rotate, the right mouse button to pan and the scroll wheel to zoom. To allow the user to move closer to the different geometries and thus hear the volume changes of the different audio.
Finally, I used Maximilian.js to mix the sounds of these instruments to form a soundtrack, which is perhaps a little noisy, but it is exactly what the band would sound like when practising their instruments.



## The Process
Throughout the course, the part I was most interested in and mastered best was using Three.js to make 3D scenes. I then started to delve into some of the other features and scope of implementation of Three.js. When I was doing my audio testing, I found Positional Audio interesting in that it could simulate real world sound effects near and far, which inspired me to do this project. Think of it as a sound recognition game that simulates real world scenes.

To implement the scene I wanted to make, first I needed to add sounds using three.js. As I was learning, I tried adding different audio patterns.

- Test 1
THREE.Audio - General Audio

      // general Audio

      // Create an AudioListener and add it to the camera
      const listener = new THREE.AudioListener();
      camera.add( listener );

      // Create a global audio source
      const sound = new THREE.Audio( listener );

      //Load a sound and set it to the buffer of the Audio object
      const audioLoader = new THREE.AudioLoader();
      audioLoader.load( 'sebo5.wav', function( buffer ) {
      	sound.setBuffer( buffer );
      	sound.setLoop( true );
      	sound.setVolume( 0.5 );
      	sound.play();
      });

- Test 2
THREE.Audio - Positional Audio

      // Positional Audio1
      var listener = new THREE.AudioListener();
      camera.add(listener);
      var sound = new THREE.PositionalAudio(listener);

      var audioLoader = new THREE.AudioLoader();
      audioLoader.load("drum.wav", function (buffer) {
        sound.setBuffer(buffer);
        sound.setRefDistance(5);
        sound.play();
      });

      // drum
      var sphere = new THREE.SphereGeometry(10, 20, 16);
      var material = new THREE.MeshPhongMaterial({ color: ("rgb(193,255,103)") });
      var drum = new THREE.Mesh(sphere, material);
    
      drum.add(sound);

After successfully adding audio to achieve the function I wanted, I found it distressing that the OrbitControls plugin given by my teacher in the course could only follow the focus of the camera. The effect I wanted to achieve was that the model could be dragged and dropped, so I started searching for information on camera plug-ins. I found that the TrackballControls plugin could achieve the interaction I wanted, so I started looking into how to replace the plugin. After searching a lot I added a web library and a new Tab which contains the source code for TrackballControls.js. It's not really clear which library worked now, but in the end it achieved the effect I wanted.

- Test 3
THREE.TrackballControls

      // trackballControls;
      var controls = new THREE.TrackballControls(camera, renderer.domElement);
      
      function createControls(camera) {
        controls.rotateSpeed = 1.0;
        controls.zoomSpeed = 1.2;
        controls.panSpeed = 1;

        controls.keys = ["KeyA", "KeyS", "KeyD"];
      }

During this time I tried adding external 3D models to replace the geometry, but in the end it didn't work, and I think it might still be a library problem. Because I wanted to add models in obj format, I needed to import the obj library and the mtl library, but I always encountered many problems in adding the libraries, so this idea was abandoned and I ended up using geometry as the sound source.

- Test 4
Loading 3D models
<!DOCTYPE html>
<html>
<head>  
  <script src = "https://cdn.rawgit.com/mrdoob/three.js/master/examples/js/loaders/OBJLoader.js"></script>
    <script src = "https://cdn.rawgit.com/mrdoob/three.js/master/examples/js/loaders/MTLLoader.js"></script>

    <meta charset="utf-8" />
    <style>
      body {
        margin: 0px;
        background-color: #000000;
        overflow: hidden;
      }
    </style>
  </head>
   <body>
<script language="javascript" type="text/javascript">
  
       //创建MTL加载器
       var mtlLoader = new THREE.MTLLoader();
       //设置文件路径
       mtlLoader.setPath('../js/models/obj/');

       //加载mtl文件
       mtlLoader.load('dianziqin.mtl', function (material) {
       //创建OBJ加载器
        var objLoader = new THREE.OBJLoader();
        //设置当前加载的纹理
        objLoader.setMaterials(material);
        objLoader.setPath('../js/models/obj/');
        objLoader.load('dianziqin.obj', function (object) {
        //添加阴影
        object.traverse(function (item) {
            if(item instanceof THREE.Mesh){
                item.castShadow = true;
                item.receiveShadow = true;
            }
          });
        //缩放
        object.scale.set(.3,.3,.3);
        scene.add(object);
    })
});
</script>
</body>
</html>





I ended up using the second week of the course to create a mix of these instruments using Maximilian.js. I loaded four samples and in addition to the audio from the three geometries I added a background sound as I wanted to make the whole scene sound more chaotic, like we would hear it in the real world. The sound was manipulated using the maxiSample.paly() function and a series of parameters were adjusted to achieve the noisy effect I wanted.

After doing the final project, I felt that I had achieved the initial functionality I wanted, but there was still room for improvement in terms of refinement. For example, writing more complex geometry, or composing a sound source from several geometries, or something like that. But for time reasons I didn't do anything too complex either, and it should be noted that my project gets stuck and can take a while to load, which is one of the reasons why I can't add too many objects, my computer is a bit overloaded. In the time to follow, I will look into more knowledge about geometry deformation, importing 3D models and if I make the scenes richer as a way to enrich my game.


## Reference 
- Three.js - Audio loading and playback

https://blog.csdn.net/u014361280/article/details/124462349?ops_request_misc=&request_id=&biz_id=102&utm_term=three.js%E5%A3%B0%E9%9F%B3&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-0-124462349.142^v67^control,201^v3^add_ask,213^v2^t3_esquery_v1&spm=1018.2226.3001.4187

- Three.js - TrackballControls

https://blog.csdn.net/qq_30100043/article/details/75000631

