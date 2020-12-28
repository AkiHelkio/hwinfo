# hwinfo

kopioi seuraava rimpsu ja pastee se powershell ikkunaan:

```
$ram = $(Get-WmiObject win32_physicalmemory | select DeviceLocator,Manufacturer,Speed,@{n="Ram (GB)";e={$_.capacity/1GB}},PartNumber)
$board = $(Get-WmiObject win32_baseboard)
$cpu = $(Get-WmiObject win32_processor)
$gpu = $(Get-WmiObject Win32_VideoController | select name, AdapterRAM,@{Expression={$_.adapterram/1MB};label="MB"})

echo "------------------------- Details -------------------------"

$data = New-Object -type PSCustomObject -Property @{
  "CPU" = $cpu.Name;
  "BoardManufacturer" = $board.Manufacturer;
  "BoardModel" = $board.Product;
  "GPU_Name" = $gpu.name;
  "GPU_AdapterRam" = $gpu.MB;
  "RAM" = $ram;
}

$data | ConvertTo-Json
```

Palauttaa tuloksena rauta speksit
