---
description: Kumpulan Script Kecil Kecilan
---

# Javascript

### Ip Calculator

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>IP Calculator</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            text-align: center;
            margin: 50px;
            background-color: #f4f4f4;
        }
        h2 {
            color: #333;
        }
        input {
            padding: 10px;
            margin-bottom: 15px;
            font-size: 16px;
        }
        button {
            padding: 12px;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 16px;
        }
        #result {
            margin-top: 20px;
            background-color: #fff;
            border: 1px solid #ddd;
            border-radius: 5px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            overflow-x: auto;
        }
        table {
            width: 100%;
            border-collapse: collapse;
        }
        th, td {
            padding: 15px;
            border-bottom: 1px solid #ddd;
            text-align: left;
        }
        th {
            background-color: #4CAF50;
            color: white;
        }
    </style>
</head>
<body>

    <h2>IP Calculator</h2>

    <label for="ipCIDR">IP Address/CIDR:</label>
    <input type="text" id="ipCIDR" placeholder="Enter IP Address/CIDR">

    <button onclick="calculateIPDetails()">Calculate</button>

    <div id="result"></div>

    <script>
        function calculateIPDetails() {
            var ipCIDR = document.getElementById("ipCIDR").value;

            if (isValidIPCIDR(ipCIDR)) {
                var ipDetails = calculateIP(ipCIDR);
                displayResults(ipDetails);
            } else {
                document.getElementById("result").innerText = "Invalid IP Address/CIDR. Please enter a valid IP/CIDR.";
            }
        }

        function isValidIPCIDR(ipCIDR) {
            var ipCIDRRegex = /^(\d{1,3}\.){3}\d{1,3}\/\d{1,2}$/;
            return ipCIDRRegex.test(ipCIDR);
        }

        function calculateIP(ipCIDR) {
            var [ipAddress, cidr] = ipCIDR.split('/');
            var subnetBits = parseInt(cidr);

            var ipArray = ipAddress.split('.');
            var networkAddress = calculateNetworkAddress(ipArray, subnetBits);
            var broadcast = calculateBroadcast(networkAddress, subnetBits);
            var hostMin = calculateHostMin(networkAddress);
            var hostMax = calculateHostMax(broadcast);
            var numHosts = calculateNumHosts(subnetBits);
            var ipClass = calculateIPClass(ipArray[0]);
            var ipType = calculateIPType(ipArray);

            return {
                ipAddress: ipAddress,
                networkAddress: networkAddress,
                broadcast: broadcast,
                hostMin: hostMin,
                hostMax: hostMax,
                subnetBits: subnetBits,
                numHosts: numHosts,
                ipClass: ipClass,
                ipType: ipType
            };
        }

        function calculateNetworkAddress(ipArray, subnetBits) {
            var networkArray = ipArray.map(function (octet, index) {
                return index < 4 - Math.ceil(subnetBits / 8) ? octet : 0;
            });

            return networkArray.join('.');
        }

        function calculateBroadcast(networkAddress, subnetBits) {
            var maxHosts = Math.pow(2, 32 - subnetBits) - 2;
            var broadcastArray = networkAddress.split('.').map(function (octet, index) {
                return index < 4 - Math.ceil(subnetBits / 8) ? octet : 255;
            });

            broadcastArray[3] += maxHosts;
            return broadcastArray.join('.');
        }

        function calculateHostMin(networkAddress) {
            var hostMinArray = networkAddress.split('.');
            hostMinArray[3] += 1;
            return hostMinArray.join('.');
        }

        function calculateHostMax(broadcast) {
            var hostMaxArray = broadcast.split('.');
            hostMaxArray[3] -= 1;
            return hostMaxArray.join('.');
        }

        function calculateNumHosts(subnetBits) {
            return Math.pow(2, 32 - subnetBits) - 2;
        }

        function calculateIPClass(firstOctet) {
            if (firstOctet >= 1 && firstOctet <= 126) {
                return 'A';
            } else if (firstOctet >= 128 && firstOctet <= 191) {
                return 'B';
            } else if (firstOctet >= 192 && firstOctet <= 223) {
                return 'C';
            } else if (firstOctet >= 224 && firstOctet <= 239) {
                return 'D';
            } else if (firstOctet >= 240 && firstOctet <= 255) {
                return 'E';
            } else {
                return 'Unknown';
            }
        }

        function calculateIPType(ipArray) {
            var privateIPRanges = [
                ['10.0.0.0', '10.255.255.255'],
                ['172.16.0.0', '172.31.255.255'],
                ['192.168.0.0', '192.168.255.255']
            ];

            var ip = ipArray.join('.');

            for (var i = 0; i < privateIPRanges.length; i++) {
                if (ip >= privateIPRanges[i][0] && ip <= privateIPRanges[i][1]) {
                    return 'Private';
                }
            }

            return 'Public';
        }

        function displayResults(ipDetails) {
            var resultDiv = document.getElementById("result");

            resultDiv.innerHTML = `
                <p>Address:   ${ipDetails.ipAddress}          ${ipToBinary(ipDetails.ipAddress)}</p>
                <p>Netmask:   ${calculateNetmask(ipDetails.subnetBits)} = ${ipDetails.subnetBits}    ${ipToBinary(calculateNetmask(ipDetails.subnetBits))}</p>
                <p>Wildcard:  ${calculateWildcard(ipDetails.subnetBits)}            ${ipToBinary(calculateWildcard(ipDetails.subnetBits))}</p>
                <br>
                <p>Network:   ${ipDetails.networkAddress}/${ipDetails.subnetBits}       ${ipToBinary(ipDetails.networkAddress)} (Class ${ipDetails.ipClass})</p>
                <p>Broadcast: ${ipDetails.broadcast}            ${ipToBinary(ipDetails.broadcast)}</p>
                <p>HostMin:   ${ipDetails.hostMin}              ${ipToBinary(ipDetails.hostMin)}</p>
                <p>HostMax:   ${ipDetails.hostMax}              ${ipToBinary(ipDetails.hostMax)}</p>
                <p>Hosts/Net: ${ipDetails.numHosts}            (${ipDetails.ipType} Internet)</p>
            `;
        }

        function calculateNetmask(subnetBits) {
            var netmaskArray = [0, 0, 0, 0];

            for (var i = 0; i < subnetBits; i++) {
                netmaskArray[Math.floor(i / 8)] += 1 << (7 - (i % 8));
            }

            return netmaskArray.join('.');
        }

function calculateWildcard(subnetBits) {
    var wildcardArray = [255, 255, 255, 255];

    for (var i = 0; i < subnetBits; i++) {
        wildcardArray[Math.floor(i / 8)] &= ~(1 << (7 - (i % 8)));
    }

    return wildcardArray.join('.');
}


        function ipToBinary(ipAddress) {
            var binaryArray = [];

            ipAddress.split('.').forEach(function (octet) {
                binaryArray.push(('00000000' + parseInt(octet).toString(2)).slice(-8));
            });

            return binaryArray.join('.');
        }
        
        
        function displayResults(ipDetails) {
            var resultDiv = document.getElementById("result");

            resultDiv.innerHTML = `
                <table>
                    <tr>
                        <th>Attribute</th>
                        <th>Value</th>
                        <th>Binary</th>
                    </tr>
                    <tr>
                        <td>Address</td>
                        <td>${ipDetails.ipAddress}</td>
                        <td>${ipToBinary(ipDetails.ipAddress)}</td>
                    </tr>
                    <tr>
                        <td>Netmask</td>
                        <td>${calculateNetmask(ipDetails.subnetBits)} = ${ipDetails.subnetBits}</td>
                        <td>${ipToBinary(calculateNetmask(ipDetails.subnetBits))}</td>
                    </tr>
                    <tr>
                        <td>Wildcard</td>
                        <td>${calculateWildcard(ipDetails.subnetBits)}</td>
                        <td>${ipToBinary(calculateWildcard(ipDetails.subnetBits))}</td>
                    </tr>
                    <tr>
                        <td>Network</td>
                        <td>${ipDetails.networkAddress}/${ipDetails.subnetBits}</td>
                        <td>${ipToBinary(ipDetails.networkAddress)} (Class ${ipDetails.ipClass})</td>
                    </tr>
                    <tr>
                        <td>Broadcast</td>
                        <td>${ipDetails.broadcast}</td>
                        <td>${ipToBinary(ipDetails.broadcast)}</td>
                    </tr>
                    <tr>
                        <td>HostMin</td>
                        <td>${ipDetails.hostMin}</td>
                        <td>${ipToBinary(ipDetails.hostMin)}</td>
                    </tr>
                    <tr>
                        <td>HostMax</td>
                        <td>${ipDetails.hostMax}</td>
                        <td>${ipToBinary(ipDetails.hostMax)}</td>
                    </tr>
                    <tr>
                        <td>Hosts/Net</td>
                        <td>${ipDetails.numHosts}</td>
                        <td>(${ipDetails.ipType} Internet)</td>
                    </tr>
                </table>
            `;
        }
    </script>

</body>
</html>

```
