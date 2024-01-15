<script lang="ts">
  import { Serialport } from 'tauri-plugin-serialport-api';

  let serialport: Serialport | undefined = undefined;
  let name: string;

  function openPort() {
    serialport = new Serialport({ portName: name, baudRate: 9600 });
    serialport
      .open()
      .then((res) => {
        console.log('open serialport', res);
      })
      .catch((err) => {
        console.error(err);
      });
  }

  function closePort() {
    serialport
      .close()
      .then((res) => {
        console.log('close serialport', res);
      })
      .catch((err) => {
        console.error(err);
      });
  }
  
  function available_ports() {
    Serialport.available_ports()
      .then((res) => {
        console.log('available_ports: ', res);
      })
      .catch((err) => {
        console.error(err);
      });
  }

  function write() {
    serialport
            .write('1234')
            .then((res) => {
              console.log('write serialport: ', res);
            })
            .catch((err) => {
              console.error(err);
            });
  }

  function writeBinary() {
    serialport
            .writeBinary(new Uint8Array([0xFF, 0xFE, 0x03, 0x06, 0x05, 0xFE, 0xFF]))
            .then((res) => {
              console.log('write binary serialport: ', res);
            })
            .catch((err) => {
              console.error(err);
            });
  }

  function read() {
    serialport
            .read({ timeout: 1000 })
            .then((res) => {
              console.log('read serialport: ', res);
            })
            .catch((err) => {
              console.error(err);
            });
  }

  function listen() {
    serialport
            .listen((data: any[]) => {
              console.log('listen serialport data: ', data);
            }, false)
            .then((res) => {
              console.log('listen serialport: ', res);
            })
            .catch((err) => {
              console.error(err);
            });
  }

  function cancelRead() {
    serialport
            .cancelRead()
            .then((res) => {
              console.log('cancel read: ', res);
            })
            .catch((err) => {
              console.error(err);
            });
  }

  function closeAll() {
    Serialport.closeAll()
            .then(() => {
              console.log('close all successful');
            })
            .catch((err) => {
              console.error(err);
            });
  }

  function forceClose() {
    Serialport.forceClose('COM4')
            .then(() => {
              console.log('force close successful!');
            })
            .catch((err) => {
              console.error(err);
            });
  }
</script>

<main class="container">
  <h1>Welcome to Tauri Serial Port Plugin!</h1>

  <div class="row">
    <a href="https://vitejs.dev" target="_blank">
      <img src="/vite.svg" class="logo vite" alt="Vite Logo" />
    </a>
    <a href="https://tauri.app" target="_blank">
      <img src="/tauri.svg" class="logo tauri" alt="Tauri Logo" />
    </a>
    <a href="https://svelte.dev" target="_blank">
      <img src="/svelte.svg" class="logo svelte" alt="Svelte Logo" />
    </a>
  </div>

  <p>
    Click on the Tauri, Vite, and Svelte logos to learn more.
  </p>

  <div class="row">
    <button on:click={available_ports}>Scan Ports</button>
  </div>

  <div class="row">
    <input type="text" placeholder="write your com port here..." bind:value={name} />
    <button on:click={openPort}>Connect</button>
    <button on:click={closePort}>Disconnect</button>
    <button on:click={write}>Write</button>
    <button on:click={writeBinary}>writeBinary</button>
  </div>
  <div class="row">
    <button on:click={read}>Read</button>
    <button on:click={listen}>listen</button>
    <button on:click={cancelRead}>cancelRead</button>
    <button on:click={closeAll}>closeAll</button>
    <button on:click={forceClose}>forceClose</button>
  </div>
</main>

<style>
  .logo.vite:hover {
    filter: drop-shadow(0 0 2em #747bff);
  }

  .logo.svelte:hover {
    filter: drop-shadow(0 0 2em #ff3e00);
  }
</style>