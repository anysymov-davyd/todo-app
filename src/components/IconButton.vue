<template>
    <div class="iconButton">
        <svg v-if="iconName === 'add'" :style="{ fill: darkMode ? 'black' : 'white' }" width="10" height="10" viewBox="0 0 10 10">
            <g clip-path="url(#clip0_8_2)">
                <path d="M10 5.71429H5.71429V10H4.28571V5.71429H0V4.28571H4.28571V0H5.71429V4.28571H10V5.71429Z"/>
            </g>
            <defs>
                <clipPath id="clip0_8_2">
                    <rect width="10" height="10" fill="white"/>
                </clipPath>
            </defs>
        </svg>
        <svg v-if="iconName === 'cancel'" :style="{ fill: darkMode ? 'black' : 'white' }" width="10" height="10" viewBox="0 0 10 10">
            <path d="M7.5 1.175L6.39001 0L1.66667 5L6.39001 10L7.5 8.825L3.89451 5L7.5 1.175Z"/>
        </svg>
        <svg v-if="iconName === 'edit'" :style="{ fill: darkMode ? 'black' : 'white' }" width="10" height="10" viewBox="0 0 10 10">
            <g clip-path="url(#clip0_1_2)">
                <path d="M0 7.91695V10H2.08304L8.22664 3.8564L6.14359 1.77336L0 7.91695ZM9.83752 2.24552C10.0542 2.02888 10.0542 1.67893 9.83752 1.46229L8.5377 0.16247C8.48632 0.110968 8.42528 0.070108 8.35808 0.0422291C8.29088 0.0143502 8.21884 0 8.14609 0C8.07334 0 8.0013 0.0143502 7.9341 0.0422291C7.8669 0.070108 7.80586 0.110968 7.75448 0.16247L6.73795 1.179L8.821 3.26204L9.83752 2.24552Z"/>
            </g>
            <defs>
                <clipPath id="clip0_1_2">
                    <rect width="10" height="10" fill="white"/>
                </clipPath>
            </defs>
        </svg>
        <svg v-if="iconName === 'delete'" :style="{ fill: darkMode ? 'black' : 'white' }" width="10" height="10" viewBox="0 0 10 10">
            <g clip-path="url(#clip0_1_4)">
                <path d="M10 1.00714L8.99286 0L5 3.99286L1.00714 0L0 1.00714L3.99286 5L0 8.99286L1.00714 10L5 6.00714L8.99286 10L10 8.99286L6.00714 5L10 1.00714Z"/>
            </g>
            <defs>
                <clipPath id="clip0_1_4">
                    <rect width="10" height="10"/>
                </clipPath>
            </defs>
        </svg>
        <svg v-if="iconName === 'apply'" :style="{ fill: darkMode ? 'black' : 'white' }" width="10" height="10" viewBox="0 0 10 10">
            <path d="M3 10L0 7V6H1L3.5 8.5L9 0H10V1L4 10H3Z"/>
        </svg>
    </div>
</template>

<script>
    export default{
        props: {
            iconName: String,
            darkMode: Boolean
        }
    }
</script>

<style>
    .iconButton{
        background-color: transparent;
        width: 10px;
        height: 10px;
        padding: 7.5px;
        border-radius: 90em;
        cursor: pointer;
        transition: .2s;
        opacity: 0.4;
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .iconButton:hover{
        background-color: rgba(180, 180, 180, 0.25);
    }

    .iconButton:active{
        background-color: rgba(180, 180, 180, 0.4);
    }
</style>