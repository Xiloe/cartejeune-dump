<template>
  <div id="app">
    <b-container>
      <div class="header">
        <!-- <h1></h1> -->
      </div>
      <div class="content">
        <!-- File upload -->
        <!-- TODO: File upload / export -->
        <div class="file-upload">
          <div class="row">
            <div class="col-sm-6">
              <b-button variant="primary" class="container-fluid">
                Importer une carte (.dmp)
              </b-button>
            </div>
            <div class="col-sm-6">
              <b-button variant="success" class="container-fluid">
                Sauvegarder la carte (.dmp)
              </b-button>
            </div>
          </div>
        </div>

        <!-- Hex view -->
        <div class="hex-view">
          <span class="uid">{{
            sector_zero.block_zero.uid.padStart(8, "0")
          }}</span>
          <span class="bcc">{{ bcc.padStart(2, "0") }}</span>
          <span class="atqa">{{
            sector_zero.block_zero.atqa.padStart(2, "0")
          }}</span>
          <span class="manufacturer-info">{{
            sector_zero.block_zero.manufacturer_info.padStart(18, "0")
          }}</span>
          <br />
          <span class="data">
            {{ sector_zero.block_one.data.padStart(32, "0") }}
            <br />
            {{ sector_zero.block_two.data.padStart(32, "0") }}
          </span>
          <br />
          <span class="keya">{{
            sector_zero.trailer.keya.padStart(12, "0")
          }}</span>
          <span class="access-conditions">{{
            sector_zero.trailer.ac.padStart(6, "0")
          }}</span>
          <span class="undefined-byte">{{
            sector_zero.trailer.ub.padStart(2, "0")
          }}</span>
          <span class="keyb">{{
            sector_zero.trailer.keyb.padStart(12, "0")
          }}</span>
          <hr />
          <span class="data">
            <span class="data-id">{{ id.padStart(8, "0") }}</span
            >{{ sector_one.block_zero.data }}
            <br />
            {{ sector_one.block_one.data }}
            <br />
            {{ sector_one.block_two.data }}
          </span>
          <br />
          <span class="keya">{{
            sector_one.trailer.keya.padStart(12, "0")
          }}</span>
          <span class="access-conditions">{{
            sector_one.trailer.ac.padStart(6, "0")
          }}</span>
          <span class="undefined-byte">{{
            sector_one.trailer.ub.padStart(2, "0")
          }}</span>
          <span class="keyb">{{
            sector_one.trailer.keyb.padStart(12, "0")
          }}</span>
        </div>

        <!-- Edit form -->
        <div class="edit-form">
          <b-form-group>
            <b-form-checkbox id="advanced-input" v-model="advanced">
              Options avancées
            </b-form-checkbox>
          </b-form-group>
          <br />
          <b-form-group>
            <b-form-group
              label="ID:"
              description="L'ID est le nombre indiqué en dessous du code barre de la carte jeune"
              label-for="id-input"
            >
              <b-form-input
                id="id-input"
                v-model="id"
                type="text"
                placeholder="FFFFFFFF"
                required
              />
            </b-form-group>
          </b-form-group>
          <!-- Advanced mode -->
          <div class="advanced" v-if="advanced">
            <hr />
            <div class="row">
              <b-form-group class="col-sm-6" label="UID:" label-for="uid-input">
                <b-form-input
                  id="uid-input"
                  v-model="sector_zero.block_zero.uid"
                  type="text"
                  placeholder="69696969"
                  required
                />
              </b-form-group>
              <b-form-group
                class="col-sm-6"
                id="bcc-group"
                label="BCC:"
                label-for="bcc-input"
              >
                <b-form-input
                  id="bcc-input"
                  v-model="bcc"
                  type="text"
                  placeholder="00"
                  disabled
                />
              </b-form-group>
            </div>
            <div class="row">
              <b-form-group
                class="col-sm-6"
                label="ATQA:"
                label-for="atqa-input"
              >
                <b-form-input
                  id="atqa-input"
                  v-model="sector_zero.block_zero.atqa"
                  type="text"
                  placeholder="8804"
                  required
                />
              </b-form-group>
              <b-form-group
                class="col-sm-6"
                id="manufacturer-group"
                label="Manufacturer infos:"
                label-for="manufacturer-input"
              >
                <b-form-input
                  id="manufacturer-input"
                  v-model="sector_zero.block_zero.manufacturer_info"
                  type="text"
                  placeholder="000000000000000000"
                  required
                />
              </b-form-group>
            </div>
            <hr />
            <div class="row">
              <b-form-group
                class="col-sm-3"
                label="KeyA (Sector 0):"
                label-for="keya-input"
              >
                <b-form-input
                  id="keya-input"
                  v-model="sector_zero.trailer.keya"
                  type="text"
                  placeholder="FFFFFFFFFFFF"
                  required
                />
              </b-form-group>
              <b-form-group
                class="col-sm-3"
                id="ac-group"
                label="Access Conditions (Sector 0):"
                label-for="ac-input"
              >
                <b-form-input
                  id="ac-input"
                  v-model="sector_zero.trailer.ac"
                  type="text"
                  placeholder="0000"
                  required
                />
              </b-form-group>
              <b-form-group
                class="col-sm-3"
                id="ub-group"
                label="Undefined byte (Sector 0):"
                label-for="ub-input"
              >
                <b-form-input
                  id="ub-input"
                  v-model="sector_zero.trailer.ub"
                  type="text"
                  placeholder="69"
                  required
                />
              </b-form-group>
              <b-form-group
                class="col-sm-3"
                id="keyb-group"
                label="KeyB (Sector 0):"
                label-for="keyb-input"
              >
                <b-form-input
                  id="keyb-input"
                  v-model="sector_zero.trailer.keyb"
                  type="text"
                  placeholder="000000000000000000"
                  required
                />
              </b-form-group>
            </div>
            <div class="row">
              <b-form-group
                class="col-sm-3"
                label="KeyA (Sector 1):"
                label-for="keya-input"
              >
                <b-form-input
                  id="keya-input"
                  v-model="sector_one.trailer.keya"
                  type="text"
                  placeholder="FFFFFFFFFFFF"
                  required
                />
              </b-form-group>
              <b-form-group
                class="col-sm-3"
                id="ac-group"
                label="Access Conditions (Sector 1):"
                label-for="ac-input"
              >
                <b-form-input
                  id="ac-input"
                  v-model="sector_one.trailer.ac"
                  type="text"
                  placeholder="0000"
                  required
                />
              </b-form-group>
              <b-form-group
                class="col-sm-3"
                id="ub-group"
                label="Undefined byte (Sector 1):"
                label-for="ub-input"
              >
                <b-form-input
                  id="ub-input"
                  v-model="sector_one.trailer.ub"
                  type="text"
                  placeholder="69"
                  required
                />
              </b-form-group>
              <b-form-group
                class="col-sm-3"
                id="keyb-group"
                label="KeyB (Sector 1):"
                label-for="keyb-input"
              >
                <b-form-input
                  id="keyb-input"
                  v-model="sector_one.trailer.keyb"
                  type="text"
                  placeholder="000000000000000000"
                  required
                />
              </b-form-group>
            </div>
          </div>
        </div>
      </div>
    </b-container>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      advanced: false,
      id: "FFFFFFFF",
      sector_zero: {
        block_zero: {
          uid: "DEADBEEF",
          atqa: "8804",
          manufacturer_info: "00C848002000000017",
        },
        block_one: {
          data: "CC010790000000000000000000000000",
        },
        block_two: {
          data: "00000000000000000000000000000000",
        },
        trailer: {
          keya: "A0A1A2A3A4A5",
          ac: "787788",
          ub: "C1",
          keyb: "F982E971CFED",
        },
      },

      sector_one: {
        block_zero: {
          data: "0419300000000000FFFFFFFF",
        },
        block_one: {
          data: "00000000000000000000000000000000",
        },
        block_two: {
          data: "00000000000000000000000000000000",
        },
        trailer: {
          keya: "E9A553102EA5",
          ac: "787788",
          ub: "00",
          keyb: "1F42AB9159EE",
        },
      },
    };
  },
  computed: {
    bcc() {
      try {
        let uid = this.sector_zero.block_zero.uid;
        let bytes = uid.match(/.{1,2}/g);

        // TODO: Clean that bs
        for (let i = 0; i < bytes.length; i++) {
          bytes[i] = "0x" + bytes[i];
        }

        if (bytes.length != 4 || bytes[3].length != 4) return "??";

        // Calculate the BCC.
        // Each byte of the UID is xored by the result of the last byte XOR, first XOR is 0
        let bcc0 = 0 ^ parseInt(bytes[0]);
        let bcc1 = bcc0 ^ parseInt(bytes[1]);
        let bcc2 = bcc1 ^ parseInt(bytes[2]);
        let bcc3 = bcc2 ^ parseInt(bytes[3]);

        return bcc3.toString(16).padStart(2, "0");
      } catch (error) {
        return "??";
      }
    },
  },
};
</script>

<style>
body {
  background: var(--bs-dark);
}

.header {
  text-align: center;
}

.header a {
  text-decoration: none;
  color: var(--bs-primary);
}

.header a:hover {
  color: var(--bs-primary);
}

.header a:visited {
  color: var(--bs-primary);
}

.hex-view {
  background: var(--bs-dark);
  border: 1px solid #000;
  padding: 15px;
  font-family: var(--bs-font-monospace);
}

.hex-view p {
  margin: 0;
  padding: 0;
}

.hex-view hr {
  background: white;
}

.hex-view .uid {
  color: var(--bs-yellow);
}

.hex-view .uid:hover {
  cursor: pointer;
  background: rgba(238, 255, 0, 0.5);
}

.hex-view .bcc {
  color: var(--bs-red);
}

.hex-view .bcc:hover {
  cursor: pointer;
  background: rgba(255, 38, 0, 0.5);
}

.hex-view .atqa {
  color: var(--bs-blue);
}

.hex-view .atqa:hover {
  cursor: pointer;
  background: rgba(0, 60, 255, 0.5);
}

.hex-view .manufacturer-info {
  color: var(--bs-purple);
}

.hex-view .manufacturer-info:hover {
  cursor: pointer;
  background: rgba(153, 1, 158, 0.5);
}

.hex-view .data {
  color: var(--bs-gray-500);
}

.hex-view .data:hover {
  cursor: pointer;
  background: rgba(126, 126, 126, 0.5);
}

.hex-view .data-id {
  color: var(--bs-pink);
}

.hex-view .data-id:hover {
  cursor: pointer;
  background: rgba(255, 0, 191, 0.5);
}

.hex-view .keya {
  color: var(--bs-teal);
}

.hex-view .keya:hover {
  cursor: pointer;
  background: rgba(0, 255, 170, 0.5);
}

.hex-view .access-conditions {
  color: var(--bs-orange);
}

.hex-view .access-conditions:hover {
  cursor: pointer;
  background: rgba(255, 102, 0, 0.5);
}

.hex-view .undefined-byte {
  color: var(--bs-green);
}

.hex-view .undefined-byte:hover {
  cursor: pointer;
  background: rgba(0, 172, 29, 0.5);
}

.hex-view .keyb {
  color: var(--bs-cyan);
}

.hex-view .keyb:hover {
  cursor: pointer;
  background: rgba(0, 185, 218, 0.5);
}

.file-upload {
  margin: 25px 0 25px 0;
}

.edit-form {
  margin: 25px 0 25px 0;
}
</style>
