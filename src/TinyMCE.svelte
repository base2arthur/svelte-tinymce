<script>
    import { onMount } from "svelte";
    import tinymce from "tinymce/tinymce";

    // Default icons are required for TinyMCE 5.3 or above
    import "tinymce/icons/default";
    import "tinymce/skins";
    // A theme is also required
    import "tinymce/skins/oxide/skin";
    import "tinymce/themes/silver/theme";
    import "tinymce/skins/ui/oxide/skin.css";
    import "tinymce/skins/ui/oxide/content.inline.css";
    // Any plugins you want to use has to be imported
    import "tinymce/plugins/paste/plugin";
    import "tinymce/plugins/link/plugin";
    import "tinymce/plugins/code/plugin";
    import "tinymce/plugins/table/plugin";
    import "tinymce/plugins/image/plugin";
    import "tinymce/plugins/insertfile/plugin";
    import "tinymce/plugins/undo/plugin";
    import "tinymce/plugins/redo/plugin";
    import "tinymce/plugins/styleselect/plugin";
    import "tinymce/plugins/bold/plugin";
    import "tinymce/plugins/italic/plugin";
    import "tinymce/plugins/alignleft/plugin";
    import "tinymce/plugins/aligncenter/plugin";
    import "tinymce/plugins/alignright/plugin";
    import "tinymce/plugins/alignjustify/plugin";
    import "tinymce/plugins/bullist/plugin";
    import "tinymce/plugins/numlist/plugin";
    import "tinymce/plugins/outdent/plugin";
    import "tinymce/plugins/indent/plugin";

    import "tinymce/plugins/advlist/plugin";
    import "tinymce/plugins/autolink/plugin";
    import "tinymce/plugins/charmap/plugin";
    import "tinymce/plugins/print/plugin";
    import "tinymce/plugins/preview/plugin";
    import "tinymce/plugins/anchor/plugin";
    import "tinymce/plugins/searchreplace/plugin";
    import "tinymce/plugins/visualblocks/plugin";
    import "tinymce/plugins/fullscreen/plugin";
    import "tinymce/plugins/media/plugin";
 
    import "tinymce/plugins/quickbars/plugin";
    import "tinymce/plugins/noneditable/plugin";
    import "tinymce/plugins/textpattern/plugin";
    import "tinymce/plugins/imagetools/plugin";
    import "tinymce/plugins/wordcount/plugin";
    import "tinymce/plugins/lists/plugin";
  
    import "tinymce/plugins/insertdatetime/plugin";    
    import "tinymce/plugins/toc/plugin";    
    import "tinymce/plugins/nonbreaking/plugin";    
    import "tinymce/plugins/pagebreak/plugin";    
    import "tinymce/plugins/hr/plugin";    
    import "tinymce/plugins/codesample/plugin";    
    import "tinymce/plugins/template/plugin";    
    import "tinymce/plugins/visualchars/plugin";        

    let el;
    export let data = {
        html: "",
    };
    let once = false;
    const id = "tiny_" + Math.random().toString(36).substr(2, 9);
    onMount(() => {
        once = true;
         
    });

    const change = ()=>{
        data.html = tinymce.get(id).getContent()
        data.text = tinymce.get(id).getContent({format:"text"})
        console.log(data.text)
    }

export const insertProperty=(text)=>{
    if(tinymce){
        tinymce.get(id).insertContent("[["+text+"]]")
    }
}

export let properties
export let propertiesReady=false

    $: if (el && once && id&&propertiesReady) {
        once = false;
        tinymce.init({
            selector: "#" + id, 

            content_css: "/tinymce/content.css",
            height: 300,
            plugins:
                "print preview paste  searchreplace autolink code visualblocks visualchars fullscreen image link media template codesample table charmap hr pagebreak nonbreaking anchor toc insertdatetime advlist lists wordcount imagetools textpattern noneditable  charmap quickbars ",
            imagetools_cors_hosts: ["picsum.photos"],
            menubar: "file custom edit view insert format tools table ",
            menu:{
                custom: { title: 'Task Properties', items:( properties?properties.join(" "):"") }
            },
            toolbar:
                "undo redo | bold italic underline strikethrough | fontselect fontsizeselect formatselect | alignleft aligncenter alignright alignjustify | outdent indent |  numlist bullist | forecolor backcolor removeformat | pagebreak | charmap | fullscreen  preview save print | insertfile image media template link anchor codesample | ltr rtl",
            toolbar_sticky: true,
            autosave_ask_before_unload: true,
            autosave_interval: "30s",
            autosave_prefix: "{path}{query}-{id}-",
            autosave_restore_when_empty: false,
            autosave_retention: "2m",
            image_advtab: true,
            
            setup: function(editor) {
                editor.on('input', function(e) {
                    change()
                });
                editor.on('blur', function(e) {
                    change()
                });
                properties?.forEach(p=>{
                    editor.ui.registry.addMenuItem(p, {
                        text: p,
                        onAction: function () {
                            editor.insertContent("[["+p+"]]");
                        }
                    });
                })
                
            },
            link_list: [
                { title: "My page 1", value: "https://www.tiny.cloud" },
                { title: "My page 2", value: "http://www.moxiecode.com" },
            ],
            image_list: [
                { title: "My page 1", value: "https://www.base2services.com" },
                { title: "My page 2", value: "http://www.moxiecode.com" },
            ],
            image_class_list: [
                { title: "None", value: "" },
                { title: "Some class", value: "class-name" },
            ],
            importcss_append: true,
            file_picker_callback: function (callback, value, meta) {
                /* Provide file and text for the link dialog */
                if (meta.filetype === "file") {
                    callback("https://base2services.com/images/base2.svg", {
                        text: "base2Services",
                    });
                }

                /* Provide image and alt text for the image dialog */
                if (meta.filetype === "image") {
                    callback("https://base2services.com/images/base2.svg", {
                        alt: "base2Services",
                    });
                }

                /* Provide alternative source and posted for the media dialog */
                if (meta.filetype === "media") {
                    callback("movie.mp4", {
                        source2: "alt.ogg",
                        poster: "https://base2services.com/images/base2.svg",
                    });
                }
            },
            templates: [
                {
                    title: "New Table",
                    description: "creates a new table",
                    content:
                        '<div class="mceTmpl"><table width="98%%"  border="0" cellspacing="0" cellpadding="0"><tr><th scope="col"> </th><th scope="col"> </th></tr><tr><td> </td><td> </td></tr></table></div>',
                },
                {
                    title: "Starting my story",
                    description: "A cure for writers block",
                    content: "Once upon a time...",
                },
                {
                    title: "New list with dates",
                    description: "New List with dates",
                    content:
                        '<div class="mceTmpl"><span class="cdate">cdate</span><br /><span class="mdate">mdate</span><h2>My List</h2><ul><li></li><li></li></ul></div>',
                },
            ],
            template_cdate_format:
                "[Date Created (CDATE): %m/%d/%Y : %H:%M:%S]",
            template_mdate_format:
                "[Date Modified (MDATE): %m/%d/%Y : %H:%M:%S]",
            height: 600,
            image_caption: true,
            quickbars_selection_toolbar:
                "bold italic | quicklink h2 h3 blockquote quickimage quicktable",
            noneditable_noneditable_class: "mceNonEditable",
            toolbar_mode: "sliding",
            contextmenu: "link image imagetools table",
       
            style_formats: [
                { title: 'Headers', items: [
                { title: 'h1', block: 'h1' },
                { title: 'h2', block: 'h2' },
                { title: 'h3', block: 'h3' },
                { title: 'h4', block: 'h4' },
                { title: 'h5', block: 'h5' },
                { title: 'h6', block: 'h6' }
                ] },

                { title: 'Blocks', items: [
                { title: 'p', block: 'p' },
                { title: 'div', block: 'div' },
                { title: 'pre', block: 'pre' }
                ] },

                { title: 'Containers', items: [
                { title: 'section', block: 'section', wrapper: true, merge_siblings: false },
                { title: 'article', block: 'article', wrapper: true, merge_siblings: false },
                { title: 'blockquote', block: 'blockquote', wrapper: true },
                { title: 'hgroup', block: 'hgroup', wrapper: true },
                { title: 'aside', block: 'aside', wrapper: true },
                { title: 'figure', block: 'figure', wrapper: true }
                ] }
            ],
            visualblocks_default_state: true,
            end_container_on_empty_block: true,
        });
    }
</script>

<textarea {id} bind:this={el}>
    {data?.html?data.html:""}
</textarea>
