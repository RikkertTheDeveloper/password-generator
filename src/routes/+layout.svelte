<script lang='ts'>
	import '@skeletonlabs/skeleton/themes/theme-seasonal.css';
	import '@skeletonlabs/skeleton/styles/all.css';
	import '../app.postcss';

    import Password from "svelte-material-icons/Lock.svelte"
    import { AppBar, LightSwitch  } from '@skeletonlabs/skeleton';
</script>

<head>
    <title>Strong Password Generator</title>
    <meta name="description" content="Making a good password is hard, but look no further! Generate a strong and reliable password instantly using this tool.">
</head>

<div class=" min-h-full min-w-full">
    <AppBar>
        <svelte:fragment slot="lead">
            <Password class="variant-soft-tertiary bg-transparent" size=20/>
            <p class="font-bold ml-2 text-soft-800">Secure Password Generator</p>
        </svelte:fragment>

        <svelte:fragment slot="trail">
            <LightSwitch/>
        </svelte:fragment>
    </AppBar>
	<slot />

    <div class="opacity-70 absolute bottom-2 left-auto right-auto w-full text-center">
            Made with ❤ by <a href="https://www.github.com/rikkertthedeveloper">Rick Arendsen</a>
    </div>
</div>
