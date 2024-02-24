<script lang="ts">
    export let getURLeSearchParams: any;
    
    import { onMount } from "svelte";
    import promptQR from 'promptpay-qr';
    import QRCode from 'qrcode';

    const promptpayGenerate = async () => {
        const promptPayQR = await promptQR(getURLeSearchParams.promptPayCode, { amount: getURLeSearchParams.price / getURLeSearchParams.members.length });
        const base64 = await QRCode.toDataURL(promptPayQR);
        const qrImage = document.querySelector('.qrImage') as HTMLDivElement;
		qrImage.innerHTML = `<img src="${base64}" class="md:h-[50vh] max-md:w-full" alt="qrcode"/>`;
    };

    onMount(async () => {
        await promptpayGenerate();
    });
</script>

<div class="qrImage flex m-10 self-center max-md:w-full" />
<p class="prompt-700 text-4xl">{getURLeSearchParams.price / getURLeSearchParams.members.length} Baht</p>
