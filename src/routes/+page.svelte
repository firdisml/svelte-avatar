<script>

    // @ts-ignore
    import { fabric } from 'fabric';
    let body_number = 0;
	  let eye_number = 0;
    let outfit_number = 0;
    let mouth_number = 0;
	let eye = [
		'https://cdn.discordapp.com/attachments/997151027226759169/999281339155103784/IMG_0341.png',
    'https://cdn.discordapp.com/attachments/997151027226759169/1000150756520886323/IMG_0366.png',
    'https://cdn.discordapp.com/attachments/997151027226759169/1001812378205290526/IMG_0439.png'
	];
	let body = [
		'https://cdn.discordapp.com/attachments/997150987586375792/999054223666847744/IMG_0256.png',
		'https://cdn.discordapp.com/attachments/997150987586375792/999054224379879454/IMG_0260.png',
    'https://cdn.discordapp.com/attachments/997150987586375792/999054224161779892/IMG_0258.png',
    'https://cdn.discordapp.com/attachments/997150987586375792/999054224593801216/IMG_0261.png',
    'https://cdn.discordapp.com/attachments/997150987586375792/999054223901720609/IMG_0259.png'
	];

  let mouth = [
    'https://cdn.discordapp.com/attachments/997151149033525288/999279007415685181/IMG_0327.png',
    'https://cdn.discordapp.com/attachments/997151149033525288/999279008149676153/IMG_0330.png',
    'https://cdn.discordapp.com/attachments/997151149033525288/999279008963371100/IMG_0333.png',
    'https://cdn.discordapp.com/attachments/997151149033525288/999279007889637418/IMG_0329.png',
    'https://cdn.discordapp.com/attachments/997151149033525288/999279007633772584/IMG_0328.png'
  ]
  
  let outfit = [
    'https://cdn.discordapp.com/attachments/987050255470194750/1000155368615325706/IMG_0367.png',
    'https://cdn.discordapp.com/attachments/987050255470194750/1000863466288058488/IMG_0411.png',
    'https://cdn.discordapp.com/attachments/987050255470194750/1001262254001504346/IMG_0424.png',
    'https://cdn.discordapp.com/attachments/987050255470194750/1004227625436336208/IMG_0508.png'
  ]

	function body_minus_number() {
		if (body_number === 0) {
			body_number = 0;
		} else {
			body_number -= 1;
		}
	}

	function eye_minus_number() {
		if (eye_number === 0) {
			eye_number = 0;
		} else {
			eye_number -= 1;
		}
	}

  function outfit_minus_number() {
		if (outfit_number === 0) {
			outfit_number = 0;
		} else {
			outfit_number -= 1;
		}
	}

  function mouth_minus_number() {
		if (mouth_number === 0) {
			mouth_number = 0;
		} else {
			mouth_number -= 1;
		}
	}


    $: urls = [body[body_number],eye[eye_number],'https://cdn.discordapp.com/attachments/997151120378048542/999060485448138803/IMG_0304.png',outfit[outfit_number],'https://cdn.discordapp.com/attachments/997151058881155197/1013379629261795460/IMG_0575.png',mouth[mouth_number]]

    console.log(urls)
    let loadedCount = 0;
    let images = [];



async function fabricImageFromURL(image_url) {                                                                          
  return new Promise(function(resolve, reject) {                                                                        
    try {                                                                                                               
      fabric.Image.fromURL(image_url, function (image) {                                                                
        resolve(image);                                                                                                 
      },{crossOrigin: "anonymous"});                                                                                                               
    } catch (error) {                                                                                                   
      reject(error);                                                                                                    
    }                                                                                                                   
  });                                                                                                                   
}

async function helper_merge() {

    for await (const result of urls){
        let img = await fabricImageFromURL(result)
        images.push(img);
        loadedCount++;
        if (loadedCount === urls.length) {
        // All images are loaded, start merging
        mergeImages();
    }
    }
    
}


function mergeImages() {
    

  // Get the maximum width and height of the images
  let maxWidth = 0;
  let maxHeight = 0;
  images.forEach(img => {
    if (img.width > maxWidth) {
      maxWidth = img.width;
    }
    if (img.height > maxHeight) {
      maxHeight = img.height;
    }
  });

  // Create a new canvas with the maximum size
  let canvas = new fabric.Canvas('canvas', { width: 2160, height: 2160 });

  // Merge the images into the canvas
  let x = 0;
  let count = 0
  images.forEach(img => {
    canvas.add(img.set({ left: 0, top: 0, zIndex: count}));
    count += 1;
    x += img.width;
  });

  canvas.backgroundColor = 'lightgray';
  canvas.discardActiveObject().renderAll();

  // Export the merged image as a data URL
  let dataURL = canvas.toDataURL();
  loadedCount = 0

  const link = document.createElement("a");
  link.href = dataURL;
  link.download = "Avatar";
  link.click();
}

</script>

<div>
	<div class="flex p-5">

    <div class="flex flex-col">
      <button on:click={body_minus_number}>◀️</button>
      <button on:click={eye_minus_number}>◀️</button>
      <button on:click={mouth_minus_number}>◀️</button>
      <button on:click={outfit_minus_number}>◀️</button>
    </div>

		<div class="relative border">
			<img src={body[body_number]} alt="" class="w-80 h-80" />
            <img src={eye[eye_number]} alt="" class="absolute top-0 left-0 z-10 w-80 h-80" />
            <img src="https://cdn.discordapp.com/attachments/997151120378048542/999060485448138803/IMG_0304.png" alt="" class="absolute top-0 left-0 z-10 w-80 h-80" />
            <img src={outfit[outfit_number]} alt="" class="absolute top-0 left-0 z-10 w-80 h-80" />
            <img src={mouth[mouth_number]} alt="" class="absolute top-0 left-0 z-10 w-80 h-80" />
			      <img src="https://cdn.discordapp.com/attachments/997151058881155197/1013379629261795460/IMG_0575.png" alt="" class="absolute top-0 left-0 z-20 w-80 h-80" />
            
		</div>

    <div class="flex flex-col">
      <button on:click={() => body_number = (body_number + 1) % body.length }>▶️</button>
      <button on:click={() => eye_number = (eye_number + 1) % eye.length }>▶️</button>
      <button on:click={() => mouth_number = (mouth_number + 1) % mouth.length }>▶️</button>
      <button on:click={() => outfit_number = (outfit_number + 1) % outfit.length }>▶️</button>
    </div>

	</div>
    <button on:click={helper_merge}>Download</button>

</div>
