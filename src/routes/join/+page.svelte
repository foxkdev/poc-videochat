<script>
  import {Peer} from 'peerjs'
var peer = new Peer('', {
  host: 'peer-server-8x76.onrender.com',
  port: 443,
  path: '/',
  secure: true,
});
let codeid = ""
// let videocurrent;
// let videoEl;
let youid = ""


let localStream = null
let localVideo = null;

let removeStream = null
let remoteVideo = null;

  // GET YOU ID
  peer.on("open",(id)=>{
    youid = id
    console.log('Conexion establecida con ID: ' + id)
    initMedia();
  })
  // IF ERROR CAN GET ID
   peer.on("error",(id)=>{
    console.log("error id "+ id)
  })

  // peer.on("connection",(conn)=>{
  //   console.log("message....")
  //   conn.on("data",(data)=>{
  //     console.log("new data " + data)
  //   })
  //   conn.on("open",()=>{
  //     console.log("new message")
  //   })
  // })
  
  // HANDLE CONNECTTION
  peer.on("call",async(call)=>{
    call.answer(localStream)
    call.on("stream", (stream) => {
      remoteVideo.srcObject = stream
    })
})

const initMedia = () => {
  navigator.mediaDevices.getUserMedia({
    video:true,
    audio:true
  }).then((stream)=>{
    // stream.getAudioTracks().forEach(track => {
    //   track.enabled = false;
    // });
    localVideo.srcObject = stream
    localVideo.muted = true
    localStream = stream
  })
}

const connectToPeer = () => {
  const call = peer.call(codeid, localStream)

  call.on('stream', (stream) => {
    // console.log(stream.getAudioTracks())
    remoteVideo.srcObject = stream
  })

}

</script>
<div>
  you id cam  = {youid}
  <br>
  code : <input type=""
  bind:value={codeid} name="">
  <!-- BUTTON CONNECT TO FRIEND -->
  <button on:click={connectToPeer}>
  connect</button>

  <!-- VIDEO YOU FRIEND TAG HTML -->
  REMOTE:
  <video 
  bind:this={remoteVideo}
  width="400" height="400" autoplay="true">
    <!-- <track kind="captions" src=""> -->
  </video>
  <br>
  YOU:
  <!-- YOU FACE CAM HERE -->
  <video 
  bind:this={localVideo}
  width="400" height="400" autoplay="true">
    <!-- <track kind="captions" src=""> -->
  </video>

  <!-- <button on:click={localVideo.muted = !localVideo.muted}> mute you</button>
  <button on:click={remoteVideo.muted = !remoteVideo.muted}>mute remote </button> -->
</div>