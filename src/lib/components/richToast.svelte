<script>
    import toast_ from "svelte-french-toast";

    /**
     * @type {{ id: string | undefined; }}
     */
    export let toast;
    /**
     * @type {object}
     */
    export let someProp;

    $: console.log({ toast, someProp, toast_ });

    // Use this component in your app:
    // toast(RichContent, { props: { someProp: '⭐' }})
</script>

<span>
    Custom and <b>bold</b> with props like {someProp}!
    <button on:click={() => toast_.dismiss(toast.id)}>Dismiss</button>
</span>
