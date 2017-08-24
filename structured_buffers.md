# Structured Buffers

# Basic

Some interesting insights at [understanding structured buffer performance](https://developer.nvidia.com/content/understanding-structured-buffer-performance).
To avoid these pitfalls in your own code, only a couple simple steps are required:

- Aim for structures with sizes divisible by 128 bits (sizeof float4)
- Pay attention to internal alignment so that vector types are ‘naturally’ aligned

[Creating structured buffer in D3D11](http://d.hatena.ne.jp/hanecci/20111126/1322332593)
```c
template <class T>
HRESULT CreateStructuredBuffer(
   ID3D11Device*               pd3dDevice,
   const UINT                  iNumElements,
   const bool                  isCpuWritable,
   const bool                  isGpuWritable,
   ID3D11Buffer**              ppBuffer,
   ID3D11ShaderResourceView**  ppSRV,
   ID3D11UnorderedAccessView** ppUAV,
   const T*                    pInitialData = NULL )
{
    HRESULT hr = S_OK;

    assert( pd3dDevice != NULL );
    assert( ppBuffer   != NULL );
    assert( ppSRV      != NULL );

    D3D11_BUFFER_DESC bufferDesc;
    ZeroMemory( &bufferDesc, sizeof(bufferDesc) );
    bufferDesc.ByteWidth = iNumElements * sizeof(T);
    
    if ( ( ! isCpuWritable ) && ( ! isGpuWritable ) ) {
        bufferDesc.CPUAccessFlags = 0;
        bufferDesc.BindFlags      = D3D11_BIND_SHADER_RESOURCE;
        bufferDesc.Usage          = D3D11_USAGE_IMMUTABLE;
    }
    else if ( isCpuWritable && ( ! isGpuWritable ) ) {
        bufferDesc.CPUAccessFlags = D3D11_CPU_ACCESS_WRITE;
        bufferDesc.BindFlags      = D3D11_BIND_SHADER_RESOURCE;
        bufferDesc.Usage          = D3D11_USAGE_DYNAMIC;
    }
    else if ( ( ! isCpuWritable ) && isGpuWritable ) {
        bufferDesc.CPUAccessFlags = 0;
        bufferDesc.BindFlags      = ( D3D11_BIND_SHADER_RESOURCE |
                                      D3D11_BIND_UNORDERED_ACCESS );
        bufferDesc.Usage          = D3D11_USAGE_DEFAULT;
    }
    else {
        assert( ( ! ( isCpuWritable && isGpuWritable ) ) );
    }

    bufferDesc.MiscFlags = D3D11_RESOURCE_MISC_BUFFER_STRUCTURED;
    bufferDesc.StructureByteStride = sizeof(T);

    D3D11_SUBRESOURCE_DATA bufferInitData;
    ZeroMemory( ( & bufferInitData), sizeof( bufferInitData ) );
    bufferInitData.pSysMem = pInitialData;
    V_RETURN( pd3dDevice->CreateBuffer( ( & bufferDesc ),
                                        ( pInitialData ) ? ( & bufferInitData ) : NULL,
                                        ppBuffer ) );

    D3D11_SHADER_RESOURCE_VIEW_DESC srvDesc;
    ZeroMemory( &srvDesc, sizeof(srvDesc) );
    srvDesc.Format              = DXGI_FORMAT_UNKNOWN;
    srvDesc.ViewDimension       = D3D11_SRV_DIMENSION_BUFFER;
    srvDesc.Buffer.ElementWidth = iNumElements;
    V_RETURN( pd3dDevice->CreateShaderResourceView( *ppBuffer, &srvDesc, ppSRV ) );

    if ( isGpuWritable ) {
        assert( ppUAV != NULL );
        D3D11_UNORDERED_ACCESS_VIEW_DESC uavDesc;
        ZeroMemory( ( & uavDesc), sizeof( uavDesc ) );
        uavDesc.Format             = DXGI_FORMAT_UNKNOWN;
        uavDesc.ViewDimension      = D3D11_UAV_DIMENSION_BUFFER;
        uavDesc.Buffer.NumElements = iNumElements;
        V_RETURN( pd3dDevice->CreateUnorderedAccessView( *ppBuffer, &uavDesc, ppUAV ) );
    }
    return hr;
}
```
