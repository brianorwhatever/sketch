<script>
  import CanvasDraw from "../src/CanvasDraw.svelte";
  let brushColor = `#${Math.floor(Math.random() * 16777215).toString(16)}`;
  let brushRadius = 10;
  let bgImage = "./images/cat.png";
  let imgBase64 = null;
  let SDraw = null;

  function setUploadedImage(e) {
    // console.log(fileUploader.files)
    console.log(e.target.files[0]);

    if (e.target.files[0]) {
      let reader = new FileReader();
      reader.readAsDataURL(e.target.files[0]);
      console.log(reader.result);
      reader.onload = (ev) => {
        // console.log(ev.target.result)
        // imgBase64 = reader.result;
        imgBase64 = ev.target.result;
        setBgImage();
      };
    }
  }

  function downloadDrawingFile(contentBase64) {
    const downloadLink = document.createElement("a");
    document.body.appendChild(downloadLink);

    downloadLink.href = contentBase64;
    downloadLink.target = "_self";
    downloadLink.download = "svelte-draw-export-" + +new Date();
    downloadLink.click();
  }

  function setBgImage() {
    bgImage = imgBase64;
  }

  function clear() {
    SDraw.clearDrawings();
  }

  function undo() {
    SDraw.undoDrawings();
  }

  function get_image_data() {
    let preparedDS = SDraw.get_image_data();
    downloadDrawingFile(preparedDS);
  }
</script>

<div class="container">
  <div class="row">
    <div class="col">
      <div class="row">
        <div class="col d-flex justify-content-center">
          <div class="col-auto">
            <h1>sketch.money</h1>
          </div>
        </div>
      </div>
      <div class="row tools">
        <div>
          <label for="color">Brush Color</label>
          <input name="color" type="color" bind:value={brushColor} />
        </div>
        <div>
          <label for="radius">Brush Radius</label>
          <input name="radius" type="number" bind:value={brushRadius} />
        </div>
        <div>
          <button class="btn btn-primary" on:click={clear}>Clear</button>
        </div>
      </div>
      <div class="row">
        <div class="col d-flex justify-content-center">
          <CanvasDraw bind:this={SDraw} {brushColor} {brushRadius} />
        </div>
      </div>
    </div>
  </div>
</div>

<style>
  .tools {
    width: 600px;
    margin: auto;
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 10px;
    align-items: end;
    justify-items: center;
  }

  .row {
    text-align: center;
  }

  h1 {
    color: #33b90e;
    text-transform: uppercase;
    font-size: 4em;
    font-weight: 300;
    margin: 0;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>
