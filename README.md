# smart_contract
hey!! here I have written smart-contract about antique verification..

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.22;

contract AntiqueVerification {
    address public owner;
    uint256 public nextAntiqueId = 1;

    struct Antique { uint256 id; string name; uint256 year; address owner; bool verified; }

    mapping(uint256 => Antique) public antiques;

    event AntiqueAdded(uint256 id, string name, uint256 year, address owner);
    event AntiqueVerified(uint256 id, address verifier);

    constructor() { owner = msg.sender; }

    function addAntique(string memory name, uint256 year) public  {
        uint256 id = nextAntiqueId++;
        antiques[id] = Antique(id, name, year, msg.sender, false);
        emit AntiqueAdded(id, name, year, msg.sender);
    }

    function verifyAntique(uint256 id) public  {
        require(antiques[id].id != 0, "Antique with this ID does not exist");
        antiques[id].verified = true;
        emit AntiqueVerified(id, msg.sender);
    }

    function getAntiqueDetails(uint256 id) public view returns (string memory, uint256, address, bool) {
        require(antiques[id].id != 0, "Antique with this ID does not exist");
        Antique memory a = antiques[id];
        return (a.name, a.year, a.owner, a.verified);
    }
}
