#  sec-practical-
# Create a voting system with multiple candidates. Each address can vote only once.
```
pragma solidity ^0.8.0;

contract Voting {
    struct Candidate {
        string name;
        uint voteCount;
    }

    Candidate[] public candidates;
    mapping(address => bool) public hasVoted;

    constructor(string[] memory candidateNames) {
        for (uint i = 0; i < candidateNames.length; i++) {
            candidates.push(Candidate(candidateNames[i], 0));
        }
    }

    function vote(uint candidateIndex) public {
        require(!hasVoted[msg.sender], "You have already voted");
        require(candidateIndex < candidates.length, "Invalid candidate index");

        candidates[candidateIndex].voteCount++;
        hasVoted[msg.sender] = true;
    }

    function getCandidates() public view returns (Candidate[] memory) {
        return candidates;
    }

    function totalCandidates() public view returns (uint) {
        return candidates.length;
    }
}
```
![image](https://github.com/user-attachments/assets/3ca00e3f-5ef9-47b8-ac4f-0dfe12202604)
![image](https://github.com/user-attachments/assets/a3d22f46-14e2-4b16-9c42-8445dd170215)


# Write a contract that manages a list of student records (name, roll number). Allow adding and retrieving student data.
```
pragma solidity ^0.8.0;

contract StudentRecords {
    struct Student {
        string name;
        uint rollNumber;
    }

    Student[] public students;

    function addStudent(string memory _name, uint _rollNumber) public {
        students.push(Student(_name, _rollNumber));
    }

    function getStudent(uint index) public view returns (string memory, uint) {
        require(index < students.length, "Invalid index");
        Student memory s = students[index];
        return (s.name, s.rollNumber);
    }

    function totalStudents() public view returns (uint) {
        return students.length;
    }
}
```
![image](https://github.com/user-attachments/assets/414c9b03-d58f-46c8-a829-d338a10deb3d)
![image](https://github.com/user-attachments/assets/33c2639e-8690-4a9d-8e27-6b7b12c93aba)


# Develop a contract that only allows the deployer (owner) to call a specific function (use modifiers).
```
pragma solidity ^0.8.0;

contract OwnerOnly {
    address public owner;

    constructor() {
        owner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Not the contract owner");
        _;
    }

    function privilegedAction() public onlyOwner {
        // Protected logic here
    }
}
```
![image](https://github.com/user-attachments/assets/d59c7cb4-d0ba-412b-b3ac-24e7f2a75eee)
![image](https://github.com/user-attachments/assets/39f24ed2-33b4-4651-a3e4-bc7c6494fd90)



# Write a contract where people can donate Ether and the top 3 donors are tracked
```
pragma solidity ^0.8.0;

contract TopDonors {
    struct Donor {
        address addr;
        uint amount;
    }

    Donor[3] public topDonors;

    function donate() public payable {
        require(msg.value > 0, "Must donate some ETH");

        // Check if already a top donor
        for (uint i = 0; i < 3; i++) {
            if (topDonors[i].addr == msg.sender) {
                topDonors[i].amount += msg.value;
                sortTopDonors();
                return;
            }
        }

        // Replace the lowest if new donor is bigger
        if (msg.value > topDonors[2].amount) {
            topDonors[2] = Donor(msg.sender, msg.value);
            sortTopDonors();
        }
    }

    function sortTopDonors() internal {
        for (uint i = 0; i < 3; i++) {
            for (uint j = i + 1; j < 3; j++) {
                if (topDonors[j].amount > topDonors[i].amount) {
                    Donor memory temp = topDonors[i];
                    topDonors[i] = topDonors[j];
                    topDonors[j] = temp;
                }
            }
        }
    }

    function getTopDonors() public view returns (Donor[3] memory) {
        return topDonors;
    }
}
```
![image](https://github.com/user-attachments/assets/3e07eb74-cbc6-4af6-bc49-23dd21e10b63)
![image](https://github.com/user-attachments/assets/a054973c-9941-48af-87ee-1e198f5b3a4b)


# Implement a simple auction system where users can place bids, and the highest bidder wins.
```
pragma solidity ^0.8.0;

contract SimpleAuction {
    address public highestBidder;
    uint public highestBid;
    address public owner;

    constructor() {
        owner = msg.sender;
    }

    function bid() public payable {
        require(msg.value > highestBid, "Bid too low");

        // Refund previous bidder
        if (highestBid > 0) {
            payable(highestBidder).transfer(highestBid);
        }

        highestBidder = msg.sender;
        highestBid = msg.value;
    }

    function endAuction() public {
        require(msg.sender == owner, "Only owner can end auction");
        payable(owner).transfer(address(this).balance);
    }
}
```
![image](https://github.com/user-attachments/assets/e713b9ad-cf8a-4297-8ae8-7c12080ec553)
![image](https://github.com/user-attachments/assets/2f1e35bf-9459-450f-913c-6519c78b1ac8)


# Create a contract that splits incoming Ether between 3 fixed addresses.
```pragma solidity ^0.8.0;

contract EtherSplitter {
    address payable public addr1;
    address payable public addr2;
    address payable public addr3;

    constructor(address payable _a1, address payable _a2, address payable _a3) {
        addr1 = _a1;
        addr2 = _a2;
        addr3 = _a3;
    }

    receive() external payable {
        uint share = msg.value / 3;
        addr1.transfer(share);
        addr2.transfer(share);
        addr3.transfer(msg.value - 2 * share); // Handle rounding
    }
}
```
![image](https://github.com/user-attachments/assets/dae8c819-6eae-444b-9ca9-052a6060fe08)
![image](https://github.com/user-attachments/assets/b43626ea-c669-4dc4-a5bb-0bc41226b59f)







