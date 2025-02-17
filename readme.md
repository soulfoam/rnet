# Rally Networking Library

RNET is an async networking library for TCP and UDP sockets. 

RNET optionally supports SSL using OpenSSL as a dependency, simply use the target `openssl` to compile and link with OpenSSL. 

## Examples

Proper docs aren't written yet but the examples show most of the functionality and will get you started.

[View the examples here](https://github.com/soulfoam/rnet-examples)

## Notes

IPv6 support isn't implemented fully yet, so it's not safe to use the `rnet_address_ipv6` API.

## Usage flow
    
```
main :: fn (args: u8&&, arg_count: usize) -> u8
{
    options: rnet_options =
    {   
        max_sockets         = 8092,
        max_writes_in_queue = 8092,

        user_data           = NULL,

        mem_alloc_fn        = NULL, 
        mem_dealloc_fn      = NULL, 
        mem_user_data       = NULL, 
    }

    err: rnet_error = rnet_setup(options&);
    if (err != RNET_OK)
    {
        std_io_println("failed to setup rnet {}", rnet_error_as_string(err));
        return 1;
    }
    
    while (true)
    {
        // `rnet_poll` will not block and returns immediately
        // `rnet_poll_until_event` will block until an event occurs 
        err = rnet_poll(); 
        if (err != RNET_OK)
        {     
            std_io_println("failed to poll {}", rnet_error_as_string(err));
            break;
        }
    }

    rnet_cleanup();

    return 0;
}
```

