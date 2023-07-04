<template>
    <link
        rel="stylesheet"
        href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css"
        integrity="sha512-iecdLmaskl7CVkqkXNQ/ZH/XLlvWZOJyj7Yy7tcenmpD1ypASozpmT/E0iPtmFIB46ZmdtAc9eNBvH0H/ZpiBw=="
        crossorigin="anonymous"
        referrerpolicy="no-referrer"
    />
    <div :style="{ backgroundColor: bgColor, width: '100%', height: '100vh' }">
        <div
            class="container-fluid d-flex justify-content-center align-items-center vh-100"
        >
            <div class="row flex-column col-md-6 col-9">
                <div
                    class="col-12 col-md-12 mb-4 d-flex justify-content-center position-relative"
                >
                    <!-- Ô input HEX -->
                    <input
                        type="text"
                        class="form-control text-center"
                        v-model="hexValue"
                        placeholder="HEX"
                    />
                    <button
                        class="copy-icon"
                        @click="copyToClipboard(hexValue)"
                    >
                        <i class="fas fa-copy"></i>
                    </button>
                </div>
                <div
                    class="col-12 col-md-12 mb-4 d-flex justify-content-center position-relative"
                >
                    <!-- Ô input RGB -->
                    <input
                        type="text"
                        class="form-control text-center"
                        v-model="rgbValue"
                        placeholder="RGB"
                    />
                    <button
                        class="copy-icon"
                        @click="copyToClipboard(rgbValue)"
                    >
                        <i class="fas fa-copy"></i>
                    </button>
                </div>
                <div
                    class="col-12 col-md-12 mb-4 d-flex justify-content-center position-relative"
                >
                    <!-- Ô input HSL -->
                    <input
                        type="text"
                        class="form-control text-center"
                        v-model="hslValue"
                        placeholder="HSL"
                    />
                    <button
                        class="copy-icon"
                        @click="copyToClipboard(hslValue)"
                    >
                        <i class="fas fa-copy"></i>
                    </button>
                </div>
                <div
                    class="col-12 col-md-12 mb-4 d-flex justify-content-center position-relative"
                >
                    <!-- Ô input CMYK -->
                    <input
                        type="text"
                        class="form-control text-center"
                        v-model="cmykValue"
                        placeholder="CMYK"
                    />
                    <button
                        class="copy-icon"
                        @click="copyToClipboard(cmykValue)"
                    >
                        <i class="fas fa-copy"></i>
                    </button>
                </div>
                <button @click="changeColor">Change Color</button>
            </div>
        </div>

        <div id="color-box"></div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            bgColor: '#111',
            hexValue: '',
            rgbValue: '',
            hslValue: '',
            cmykValue: '',
        }
    },
    methods: {
        changeColor() {
            var randomColor = getRandomColor()
            this.bgColor = randomColor
            this.hexValue = randomColor
            this.rgbValue = convertToRGB(randomColor)
            this.hslValue = convertToHSL(randomColor)
            this.cmykValue = convertToCMYK(randomColor)
        },
        copyToClipboard(value) {
            const el = document.createElement('textarea')
            el.value = value
            document.body.appendChild(el)
            el.select()
            document.execCommand('copy')
            document.body.removeChild(el)
        },
    },
}

function convertToRGB(color) {
    let hex = color.slice(1)
    let r = parseInt(hex.substring(0, 2), 16)
    let g = parseInt(hex.substring(2, 4), 16)
    let b = parseInt(hex.substring(4, 6), 16)
    return `rgb(${r}, ${g}, ${b})`
}

function convertToHSL(color) {
    let hex = color.slice(1)
    let r = parseInt(hex.substring(0, 2), 16) / 255
    let g = parseInt(hex.substring(2, 4), 16) / 255
    let b = parseInt(hex.substring(4, 6), 16) / 255

    let max = Math.max(r, g, b)
    let min = Math.min(r, g, b)
    let h,
        s,
        l = (max + min) / 2

    if (max == min) {
        h = s = 0
    } else {
        let d = max - min
        s = l > 0.5 ? d / (2 - max - min) : d / (max + min)

        switch (max) {
            case r:
                h = (g - b) / d + (g < b ? 6 : 0)
                break
            case g:
                h = (b - r) / d + 2
                break
            case b:
                h = (r - g) / d + 4
                break
        }

        h /= 6
    }

    h = Math.round(h * 360)
    s = Math.round(s * 100)
    l = Math.round(l * 100)

    return `hsl(${h}, ${s}%, ${l}%)`
}

function convertToCMYK(color) {
    let hex = color.slice(1)
    let r = parseInt(hex.substring(0, 2), 16) / 255
    let g = parseInt(hex.substring(2, 4), 16) / 255
    let b = parseInt(hex.substring(4, 6), 16) / 255

    let k = 1 - Math.max(r, g, b)
    let c = (1 - r - k) / (1 - k)
    let m = (1 - g - k) / (1 - k)
    let y = (1 - b - k) / (1 - k)

    c = Math.round(c * 100)
    m = Math.round(m * 100)
    y = Math.round(y * 100)
    k = Math.round(k * 100)

    return `cmyk(${c}%, ${m}%, ${y}%, ${k}%)`
}

function getRandomColor() {
    var r = Math.floor(Math.random() * 256)
    var g = Math.floor(Math.random() * 256)
    var b = Math.floor(Math.random() * 256)
    return `#${r.toString(16)}${g.toString(16)}${b.toString(16)}`
}
</script>
<style scoped>
.copy-icon {
    position: absolute;
    top: 50%;
    right: 10px;
    transform: translateY(-50%);
    background: none;
    border: none;
    cursor: pointer;
}
</style>
