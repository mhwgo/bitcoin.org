{% comment %}
This file is licensed under the MIT License (MIT) available on
http://opensource.org/licenses/MIT.
{% endcomment %}
{% assign filename="_includes/ref/bitcoin-core/rpcs/rpcs/getblock.md" %}

##### GetBlock
{% include helpers/subhead-links.md %}

{% assign summary_getBlock="gets a block with a particular header hash from the local block database either as a JSON object or as a serialized block." %}

{% autocrossref %}

The `getblock` RPC {{summary_getBlock}}

*Parameter #1---header hash*

| Name             | Type         | Presence                    | Description
|------------------|--------------|-----------------------------|----------------
| Header Hash      | string (hex) | Required<br>(exactly 1)     | The hash of the header of the block to get, encoded as hex in RPC byte order
{:.ntpd}

*Parameter #2---JSON or hex output*

| Name             | Type         | Presence                    | Description
|------------------|--------------|-----------------------------|----------------
| Format           | bool         | Optional<br>(0 or 1)        | Set to `false` to get the block in serialized block format; set to `true` (the default) to get the decoded block as a JSON object
{:.ntpd}

*Result (if format was `false`)---a serialized block*

| Name             | Type              | Presence                    | Description
|------------------|-------------------|-----------------------------|----------------
| `result`         | string (hex)/null | Required<br>(exactly 1)     | The requested block as a serialized block, encoded as hex, or JSON `null` if an error occurred
{:.ntpd}

*Result (if format was `true` or omitted)---a JSON block*

| Name                       | Type            | Presence                    | Description
|----------------------------|-------------------|---------------------------|----------------
| `result`                   | object/null       | Required<br>(exactly 1)   | An object containing the requested block, or JSON `null` if an error occurred
| →<br>`hash`              | string (hex)      | Required<br>(exactly 1)     | The hash of this block's block header encoded as hex in RPC byte order.  This is the same as the hash provided in parameter #1
| →<br>`confirmations`     | number (int)      | Required<br>(exactly 1)     | The number of confirmations the transactions in this block have, starting at 1 when this block is at the tip of the best block chain.  This score will be -1 if the the block is not part of the best block chain
| →<br>`size`              | number (int)      | Required<br>(exactly 1)     | The size of this block in serialized block format, counted in bytes
| →<br>`height`            | number (int)      | Required<br>(exactly 1)     | The height of this block on its block chain
| →<br>`version`           | number (int)      | Required<br>(exactly 1)     | This block's version number.  See [block version numbers][section block versions]
| →<br>`merkleroot`        | string (hex)      | Required<br>(exactly 1)     | The merkle root for this block, encoded as hex in RPC byte order
| →<br>`tx`                | array             | Required<br>(exactly 1)     | An array containing the TXIDs of all transactions in this block.  The transactions appear in the array in the same order they appear in the serialized block
| → →<br>TXID              | string (hex)      | Required<br>(1 or more)     | The TXID of a transaction in this block, encoded as hex in RPC byte order
| →<br>`time`              | number (int)      | Required<br>(exactly 1)     | The value of the *time* field in the block header, indicating approximately when the block was created
| →<br>`nonce`             | number (int)      | Required<br>(exactly 1)     | The nonce which was successful at turning this particular block into one that could be added to the best block chain
| →<br>`bits`              | string (hex)      | Required<br>(exactly 1)     | The value of the *nBits* field in the block header, indicating the target threshold this block's header had to pass
| →<br>`difficulty`        | number (real)     | Required<br>(exactly 1)     | The estimated amount of work done to find this block relative to the estimated amount of work done to find block 0
| →<br>`chainwork`         | string (hex)      | Required<br>(exactly 1)     | The estimated number of block header hashes miners had to check from the genesis block to this block, encoded as big-endian hex
| →<br>`previousblockhash` | string (hex)      | Required<br>(exactly 1)     | The hash of the header of the previous block, encoded as hex in RPC byte order
| →<br>`nextblockhash`     | string (hex)      | Optional<br>(0 or 1)        | The hash of the next block on the best block chain, if known, encoded as hex in RPC byte order
{:.ntpd}

*Example from Bitcoin Core 0.10.0*

Get a block in raw hex:

{% highlight bash %}
bitcoin-cli -testnet getblock \
            000000000fe549a89848c76070d4132872cfb6efe5315d01d7ef77e4900f2d39 \
            false
{% endhighlight %}

Result (wrapped):

{% highlight text %}
02000000df11c014a8d798395b5059c722ebdf3171a4217ead71bf6e0e99f4c7\
000000004a6f6a2db225c81e77773f6f0457bcb05865a94900ed11356d0b7522\
8efb38c7785d6053ffff001d005d437001010000000100000000000000000000\
00000000000000000000000000000000000000000000ffffffff0d03b4770301\
64062f503253482fffffffff0100f9029500000000232103adb7d8ef6b63de74\
313e0cd4e07670d09a169b13e4eda2d650f529332c47646dac00000000
{% endhighlight %}

Get the same block in JSON:

{% highlight bash %}
bitcoin-cli -testnet getblock \
            000000000fe549a89848c76070d4132872cfb6efe5315d01d7ef77e4900f2d39 \
            true
{% endhighlight %}

Result:

{% highlight json %}
{
    "hash" : "000000000fe549a89848c76070d4132872cfb6efe5315d01d7ef77e4900f2d39",
    "confirmations" : 88029,
    "size" : 189,
    "height" : 227252,
    "version" : 2,
    "merkleroot" : "c738fb8e22750b6d3511ed0049a96558b0bc57046f3f77771ec825b22d6a6f4a",
    "tx" : [
        "c738fb8e22750b6d3511ed0049a96558b0bc57046f3f77771ec825b22d6a6f4a"
    ],
    "time" : 1398824312,
    "nonce" : 1883462912,
    "bits" : "1d00ffff",
    "difficulty" : 1.00000000,
    "chainwork" : "000000000000000000000000000000000000000000000000083ada4a4009841a",
    "previousblockhash" : "00000000c7f4990e6ebf71ad7e21a47131dfeb22c759505b3998d7a814c011df",
    "nextblockhash" : "00000000afe1928529ac766f1237657819a11cfcc8ca6d67f119e868ed5b6188"
}
{% endhighlight %}

*See also*

* [GetBlockHash][rpc getblockhash]: {{summary_getBlockHash}}
* [GetBestBlockHash][rpc getbestblockhash]: {{summary_getBestBlockHash}}

{% endautocrossref %}