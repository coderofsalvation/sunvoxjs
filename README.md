# sunvoxjs

<img src="http://www.warmplace.ru/soft/sunvox/disk.png"/>

This is a mirror of Alexander Zolotov's sunvoxjs lib, so it can be served over CORS (unpkg.com)

## Usage

    <script src="https://unpkg.com/sunvoxjs/sunvox.js"/>
    <script src="https://unpkg.com/sunvoxjs/sunvox_lib_loader.js"/>

## Demo / Docs

[http://warmplace.ru/soft/sunvox/jsplay/](http://warmplace.ru/soft/sunvox/jsplay/)

## Example


    <script>
        var sfile = "https://unpkg.com/sunvoxjs/test.sunvox"

        function status(l){ console.log(l) }

        function loadFromArrayBuffer( buf )
        {
            if( buf ) 
            {
                var byteArray = new Uint8Array( buf );
                if( sv_load_from_memory( 0, byteArray ) == 0 )
                {
                    console.log( "song loaded" )
                    sv_play_from_beginning( 0 );
                }
            }
            else
            {
                console.log( "song load error" );
            }
            
        }

        function loadSong(fname){
            console.log( "loading: " + fname );
            var req = new XMLHttpRequest();
            req.open( "GET", fname, true );
            req.responseType = "arraybuffer";
            req.onload = function( e ) {
                if( this.status != 200 ){
                    console.log( "file not found" );
                    return;
                }
                var arrayBuffer = this.response;
                loadFromArrayBuffer( arrayBuffer );
            };
            req.send( null ); 
        }

        svlib.then( function(Module) {
            //
            // SunVox Library was successfully loaded.
            // Here we can perform some initialization:
            //
            status( "SunVoxLib loading is complete" );
            var ver = sv_init( 0, 44100, 2, 0 ); //Global sound system init
            if( ver >= 0 )
            {
            //Show information about the library:
                var major = ( ver >> 16 ) & 255;
                var minor1 = ( ver >> 8 ) & 255;
                var minor2 = ( ver ) & 255;
                console.log( "SunVox lib version: " + major + " " + minor1 + " " + minor2 );
            console.log( "init ok" );
            }
            else
            {
            console.log( "init error" );
            return;
            }
            sv_open_slot( 0 ); //Open sound slot 0 for SunVox; you can use several slots simultaneously (each slot with its own SunVox engine)
            //
            // Try to load and play some SunVox file:
            //
            loadSong( sfile );
        });
    </script>
