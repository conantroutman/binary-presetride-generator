<template>
  <Navbar />

  <section class="section">
    <div class="columns">
      <div class="column">
        <div class="field is-horizontal">
          <div class="field-label is-normal">
            <label class="label" for="preset_name">Preset Name</label>
          </div>
          <div class="field-body">
            <div class="field">
              <div class="control">
                <input class="input" type="text" id="preset_name" v-model="presetName" placeholder="Enter a preset name">
              </div>
            </div>
          </div>
        </div>

        <div class="field is-horizontal">
          <div class="field-label is-normal">
            <label for="" class="label">Car</label>
          </div>
          <div class="field-body">
            <div class="field">
              <div class="control">
                <div class="select">
                  <select name="carselect" id="car_select" v-model="selectedCar">
                    <option :value="{}" disabled selected>Select a Car</option>
                    <option v-for="car in cars.cars" v-bind:key="car" :value="car">{{ car.name }}</option>
                  </select>
                </div>
              </div>
            </div>
          </div>
        </div>

        <div class="field is-horizontal">
          <div class="field-label is-normal">
            <label for="" class="label">Body Kit</label>
          </div>
          <div class="field-body">
            <div class="field">
              <div class="control">
                <div class="select">
                  <select id="bodykit_select" v-model="preset.bodykit">
                    <option value="0" selected>Stock</option>
                    <option v-for="(bodykit, index) in currentSelection.bodykits" v-bind:key="bodykit" :value="index+1">Body {{ index+1 }}</option>
                  </select>
                </div>
              </div>
            </div>
          </div>
        </div>
        
        <div class="field is-horizontal">
          <div class="field-label is-normal">
            <label for="" class="label">Rims</label>
          </div>
          <div class="field-body">
            <div class="field is-grouped">
              <div class="control">
                <div class="select">
                  <select name="wheelbrandselect" id="wheelbrand_select" v-model="preset.wheels.brand">
                    <option value="stock" selected>Stock</option>
                    <option v-for="brand in parts.wheels" v-bind:key="brand" :value="brand.id">{{ brand.name }}</option>
                  </select>
                </div>
              </div>
              <div class="control" v-if="preset.wheels.brand != 'stock'">
                <div class="select">
                  <select name="wheelstyleselect" id="wheelstyle_select" v-model="preset.wheels.style">
                    <option v-for="(style, index) in wheelStyles" v-bind:key="style" :value="index+1">{{ style }}</option>
                  </select>
                </div>
              </div>
              <div class="control" v-if="preset.wheels.brand != 'stock'">
                <input type="range" name="wheelsizeselect" id="wheelsize_select" min="17" max="20" step="1" v-model="preset.wheels.size">
              </div>
            </div>
          </div>
        </div>

        <div class="field is-horizontal">
          <div class="field-label is-normal">
            <label for="" class="label">Hood</label>
          </div>
          <div class="field-body">
            <div class="field is-grouped">
              <div class="control">
                <div class="select">
                  <select name="hoodselect" id="hood_select" v-model="preset.hood.style">
                    <option value="0" selected>Stock</option>
                    <option v-for="hood in parts.hoods" v-bind:key="hood" :value="hood.id">{{ hood.name }}</option>
                  </select>
                </div>
              </div>
              <div class="control is-normal" v-if="preset.hood.style > 0">
                <label class="radio" for="hood_standard">
                  <input class="radio" type="radio" name="hoodcf" id="hood_standard" :value="false" v-model="preset.hood.carbon" checked>
                  Standard
                </label>
                
                <label class="radio" for="hood_carbon">
                  <input class="radio" type="radio" name="hoodcf" id="hood_carbon" :value="true" v-model="preset.hood.carbon">
                  Carbon-Fiber
                </label>
              </div>
            </div>
          </div>
        </div>

        <div class="field is-horizontal">
          <div class="field-label is-normal">
            <label for="" class="label">Roof Scoop</label>
          </div>
          <div class="field-body">
            <div class="field is-grouped">
              <div class="control">
                <div class="select">
                  <select name="roofscoopselect" id="roofscoop_select" v-model="preset.roofscoop.style">
                    <option value="0" selected>Stock</option>
                    <option v-for="roofscoop in parts.roofscoops" v-bind:key="roofscoop" :value="roofscoop.id">{{ roofscoop.name }}</option>
                  </select>
                </div>
              </div>
              <div class="control is-expanded" v-if="preset.roofscoop.style > 0">
                <label class="radio" for="roofscoop_standard">
                  <input type="radio" name="roofscoopcf" id="roofscoop_standard" :value="false" v-model="preset.roofscoop.carbon" checked>
                  Standard
                </label>
                
                <label class="radio" for="roofscoop_carbon">
                  <input type="radio" name="roofscoopcf" id="roofscoop_carbon" :value="true" v-model="preset.roofscoop.carbon">
                  Carbon-Fiber
                </label>
              </div>
              <div class="control is-expanded" v-if="preset.roofscoop.style > 0">
                <label class="radio" for="roofscoop_centered">
                  <input type="radio" name="roofscoopvariant" id="roofscoop_centered" value="single" v-model="roofScoop" checked>
                  Centered
                </label>
                
                <label class="radio" for="roofscoop_offset">
                  <input type="radio" name="roofscoopvariant" id="roofscoop_offset" value="offset" v-model="roofScoop">
                  Offset
                </label>

                <label class="radio" for="roofscoop_dual">
                  <input type="radio" name="roofscoopvariant" id="roofscoop_dual" value="dual" v-model="roofScoop">
                  Dual
                </label>
              </div>
            </div>
          </div>
        </div>

        <div class="field is-horizontal">
          <div class="field-label is-normal">
            <label class="label" for="spoiler_select">Spoiler</label>
          </div>
          <div class="field-body">
            <div class="field is-grouped">
              <div class="control">
                <div class="select">
                  <select name="spoilerselect" id="spoiler_select" v-model="preset.spoiler.style">
                    <option value="0" selected>Stock</option>
                    <option v-for="spoiler in parts.spoilers" v-bind:key="spoiler" :value="spoiler.id">{{ spoiler.name }}</option>
                  </select>
                </div>
              </div>
              <div class="control" v-if="preset.spoiler.style > 0">
                <label for="regular_spoiler" class="radio">
                  <input type="radio" name="spoilercf" id="regular_spoiler" :value="false" v-model="preset.spoiler.carbon" checked>
                  Standard
                </label>

                <label for="carbon_spoiler" class="radio">
                  <input type="radio" name="spoilercf" id="carbon_spoiler" :value="true" v-model="preset.spoiler.carbon">
                  Carbon-Fiber
                </label>
              </div>
            </div>
          </div>
        </div>
        
        <div class="field is-horizontal">
          <div class="field-label is-normal">
            <label class="label" for="tint_select">Window Tint</label>
          </div>
          <div class="field-body">
            <div class="field">
              <div class="control">
                <div class="select">
                  <select name="tint" id="tint_select" v-model="preset.tint">
                    <option value="" selected>Stock</option>
                    <option v-for="tint in parts.tints" v-bind:key="tint" :value="tint.id">{{ tint.name }}</option>
                  </select>
                </div>
              </div>
            </div>
          </div>
        </div>

        <div class="field is-horizontal">
          <div class="field-label is-normal">
            <label class="label" for="paint_select">Paint</label>
          </div>
          <div class="field-body">
            <div class="field">
              <div class="control">
                <div class="select">
                  <select name="paint" id="paint_select" v-model="preset.paint">
                    <option value="" disabled selected="selected">Select an Option</option>
                    <option v-for="(paint) in colors.gloss" v-bind:key="paint" :value="paint.id">{{ paint.id }}</option>
                  </select>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div class="column">
        <div class="field">
          <textarea class="textarea" name="" id="" readonly rows="40" v-model="endScript"></textarea>
        </div>
        <div class="field">
          <button class="button is-primary" @click="copyEndScript">Copy</button>
        </div>
      </div>
    </div>
  </section>

  <PaintPicker />

  <Footer />
</template>

<script>

import carsJSON from './cars.json'
import partsJSON from './parts.json'
import colorsJSON from './colors.json'

import Navbar from './components/Navbar'
import Footer from './components/Footer'
import PaintPicker from './components/PaintPicker'

export default {
  name: 'App',
  components: {
    //bulmaselect,
    Navbar,
    Footer,
    PaintPicker
  },
  data() {
    return {
      cars: carsJSON,
      parts: partsJSON,
      colors: colorsJSON,
      currentSelection: {},
      preset: {
        name: '',
        car: '',
        bodykit: '0',
        wheels: {
          brand: 'stock',
          style: '1',
          size: '20'
        },
        hood: {
          style: '0',
          carbon: false
        },
        roofscoop: {
          style: '0',
          offset: false,
          dual: false,
          carbon: false
        },
        spoiler: {
          style: '0',
          carbon: false
        },
        tint: '',
        paint: '',
        wheelpaint: '',
        vinyl: {
          style: ''
        },
        decals: {
          window: {
            front: '',
            rear: ''
          },
          doors: {
            left: {
              slot1: '',
              slot2: '',
              slot3: '',
              slot4: '',
              slot5: ''
            },
            right: {
              slot1: '',
              slot2: '',
              slot3: '',
              slot4: '',
              slot5: ''
            }
          },
          rear: {
            left: '',
            right: ''
          }
        },
        number: {
          left: '',
          right: ''
        }
      },
    }
  },
  computed: {
    // End script output generation
    endScript() {
      const update = `update_collection GLOBAL\\GLOBALB.LZC PresetRides ${this.preset.name}`
      return `add_collection GLOBAL\\GLOBALB.LZC PresetRides ${this.preset.name}
${update} Frontend ${this.preset.car.toLowerCase()}
${update} MODEL ${this.preset.car.toUpperCase()}
${update} Pvehicle ${this.preset.car.toLowerCase()}
${update} AftermarketBodykit ${this.preset.car.toUpperCase()}_BODY_KIT0${this.preset.bodykit}
${update} Base ${this.preset.car.toUpperCase()}_BASE
${update} Driver ${this.preset.car.toUpperCase()}_DRIVER
${update} FrontBrake ${this.preset.car.toUpperCase()}_FRONT_BRAKE
${update} FrontLeftWindow ${this.preset.car.toUpperCase()}_FRONT_LEFT_WINDOW
${update} FrontRightWindow ${this.preset.car.toUpperCase()}_FRONT_RIGHT_WINDOW
${update} FrontWheel ${this.wheel}
${update} FrontWindow ${this.preset.car.toUpperCase()}_FRONT_WINDOW
${update} Hood ${this.preset.car.toUpperCase()}${this.preset.hood.style > 0 ? '_STYLE' + (this.preset.hood.style.length > 1 ? this.preset.hood.style : '0' + this.preset.hood.style) : '_KIT00'}_HOOD${this.preset.hood.carbon && this.preset.hood.style != '0' ? '_CF' : ''}
${update} Interior ${this.preset.car.toUpperCase()}_INTERIOR
${update} LeftBrakelight ${this.preset.car.toUpperCase()}_LEFT_BRAKELIGHT
${update} LeftBrakelightGlass ${this.preset.car.toUpperCase()}_LEFT_BRAKELIGHT_GLASS
${update} LeftHeadlight ${this.preset.car.toUpperCase()}_LEFT_HEADLIGHT
${update} LeftHeadlightGlass ${this.preset.car.toUpperCase()}_LEFT_HEADLIGHT_GLASS
${update} LeftSideMirror ${this.preset.car.toUpperCase()}_LEFT_SIDE_MIRROR
${update} LicensePlate LINCENSE_PLATE_STYLE01
${update} RearBrake ${this.preset.car.toUpperCase()}_REAR_BRAKE
${update} RearLeftWindow ${this.preset.car.toUpperCase()}_REAR_LEFT_WINDOW
${update} RearRightWindow ${this.preset.car.toUpperCase()}_REAR_RIGHT_WINDOW
${update} RearWindow ${this.preset.car.toUpperCase()}_REAR_WINDOW
${update} RightBrakelight ${this.preset.car.toUpperCase()}_RIGHT_BRAKELIGHT
${update} RightBrakelightGlass ${this.preset.car.toUpperCase()}_RIGHT_BRAKELIGHT_GLASS
${update} RightHeadlight ${this.preset.car.toUpperCase()}_RIGHT_HEADLIGHT
${update} RightHeadlightGlass ${this.preset.car.toUpperCase()}_RIGHT_HEADLIGHT_GLASS
${update} RightSideMirror ${this.preset.car.toUpperCase()}_RIGHT_SIDE_MIRROR
${update} RoofScoop ROOF_STYLE${this.preset.roofscoop.style.length > 1 ? this.preset.roofscoop.style : '0' + this.preset.roofscoop.style}${this.preset.roofscoop.offset ? '_OFFSET' : ''}${this.preset.roofscoop.dual ? '_DUAL' : ''}${this.preset.roofscoop.carbon && this.preset.roofscoop.style != '0' ? '_CF' : ''}
${update} Spoiler ${this.preset.spoiler.style > 0 ? (`SPOILER_STYLE${this.preset.spoiler.style.length > 1 ? this.preset.spoiler.style : ('0' + this.preset.spoiler.style)}${this.spoilerType}${this.preset.spoiler.carbon ? '_CF' : ''}`) : `${this.preset.car.toUpperCase()}_SPOILER`}
${update} UniversalSpoilerBase ${this.preset.car.toUpperCase()}_UNIVERSAL_SPOILER_BASE
${update} WindshieldTint ${this.preset.tint != 0 ? 'WINDSHIELD_' + this.preset.tint : 'WINDOW_TINT_STOCK'}
${update} Visuals VISUAL_SETS BasePaintType ${this.preset.paint}
${update} Visuals VISUAL_SETS RimsPaintType TEST`
    },

    wheel: {
      get() {
        return this.preset.wheels.brand == 'stock' ? `${this.preset.car.toLocaleUpperCase()}_WHEEL` : `${this.preset.wheels.brand.toUpperCase()}_STYLE0${this.preset.wheels.style}_${this.preset.wheels.size}_25`
      }
    },
    
    wheelStyles: {
      get() {
        const index = this.parts.wheels.findIndex(brand => brand.id === this.preset.wheels.brand)
        return this.parts.wheels[index].styles
      }
    },

    roofScoop: {
      set(type) {
        switch(type) {
          case 'single':
            this.preset.roofscoop.offset = false
            this.preset.roofscoop.dual = false
            break
          case 'offset':
            this.preset.roofscoop.offset = true
            this.preset.roofscoop.dual = false
            break
          case 'dual':
            this.preset.roofscoop.offset = false
            this.preset.roofscoop.dual = true
            break
        }
      }
    },

    spoilerType: {
      get() {
        let string
        switch(this.currentSelection.spoilerType) {
          case 1:
            string = '_HATCH'
            break
          case 2:
            string = '_PORSCHES'
            break
          case 3:
            string = '_CARRERA'
            break
          default:
            string = ''
            break
        }
        return string
      }
    },

    selectedCar: {
      get() {
        return this.currentSelection
      },
      set(newCar) {
        this.currentSelection = newCar
        //this.preset.name = this.currentSelection.name
        this.preset.car = this.currentSelection.id
      }
    },

    presetName: {
      get() {
        return this.preset.name
      },
      set(newName) {
        this.preset.name = newName.split(' ').join('_').toUpperCase()
      }
    }
  },
  methods: {
    copyEndScript() {
      const textarea = document.querySelector('textarea')
      textarea.select()
      document.execCommand('copy')
    }
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

textarea {
  resize: none;
}
</style>
