<script>
    export let aria;
    export let layout;
    export let name;
    export let onchange;
    export let options;
    export let disallowAllUnchecked = false;
    let checked = new Set();
    window.checked = checked;
    let disabled = [];
    function setAttributes(node, {option, i}){
        node.optionIndex = i;
        if (option.isChecked){
            checked.add(node);
        }
        disabled[i] = false;
        if (aria){
            Object.keys(aria).forEach(key => {
                if ( key !== 'label'){
                    node.setAttribute(`aria-${key}`, aria.key);
                }
            });
        }
        Object.keys(option).forEach(key => {
            node.dataset[key] = option[key];
        });
    }
    function checkDisable(){
        if (checked.size == 1){
            disabled[[...checked][0].optionIndex] = true;
            disabled = disabled;
        } else {
            disabled = disabled.map(() => false);
        }
    }
    function _onchange(e){
        onchange.call(this,e);
        if ( this.checked ){
            checked.add(this);
        } else {
            checked.delete(this);
        }
        if (disallowAllUnchecked) {
            checkDisable();
        }
    }
</script>
<style>
    .horizontal label {
        margin-right: 1em;
    }
    label {
        display: inline-flex;
        align-items: center;
    }
    input {
        appearance: none;
        -webkit-appearance: none;
        -moz-appearance: none;
        position: relative;
        width: 12px;
        height: 12px;
        border: 1px solid #296ec3;
        border-radius: 2px;
        margin-right: 0.5em;
    }
    input[disabled] {
        cursor: not-allowed;
    }
    input[disabled]:checked {
        background-color: gray;
    }
    input:focus {
        outline: 1px dotted gray;
    }
    input:checked {
        background-color: #296ec3;
    }
    input:checked::before {
        position: absolute;
        left: 0;
        top: 0;
        width: 100%;
        height: 100%;
        content: "✔";
        font-size: .625rem;
        line-height: 1;
        color: #fff;
        text-align: center;
    }
    .visually-hidden {
        position: absolute !important;
        clip: rect(1px 1px 1px 1px); /* IE6, IE7 */
        clip: rect(1px, 1px, 1px, 1px);
        padding:0 !important;
        border:0 !important;
        height: 1px !important; 
        width: 1px !important; 
        overflow: hidden;
}
</style>
<div class="{layout}">
    {#if aria.label}
    <p class="visually-hidden">{aria.label}</p>
    {/if}
    {#each options as option, i}
        <label><input disabled="{disabled[i]}" checked="{option.isChecked}" on:change="{_onchange}" {name} use:setAttributes="{{option, i}}" type="checkbox" value={option.value}> { option.display}</label>
    {/each}
</div>