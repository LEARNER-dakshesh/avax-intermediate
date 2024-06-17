Voting Smart Contract
This is a simple voting smart contract implemented in Solidity. It demonstrates the use of require(), assert(), and revert() statements to enforce various conditions and ensure the contract's correct operation.

Features
Voting Mechanism: Users can cast a vote if they haven't voted already and if the voting session is active.
End Voting: The contract owner can end the voting session.
Reset Voting: The contract owner can reset the voting session if it has ended.
Contract Details
Variables
address public owner: Stores the address of the contract owner.
bool public votingEnded: Indicates whether the voting session has ended.
mapping(address => bool) public hasVoted: Keeps track of which addresses have voted.
uint public totalVotes: Counts the total number of votes.
Modifiers
onlyOwner: Ensures that only the contract owner can call the function.

solidity
Copy code
modifier onlyOwner() {
    require(msg.sender == owner, "Only the owner can perform this action");
    _;
}
votingNotEnded: Ensures that the voting session is still active.

solidity
Copy code
modifier votingNotEnded() {
    require(!votingEnded, "Voting has ended");
    _;
}
Functions
Constructor

Initializes the contract by setting the owner and marking the voting session as active.

solidity
Copy code
constructor() {
    owner = msg.sender;
    votingEnded = false;
}
vote

Allows a user to cast a vote if they haven't already and if the voting session is active.

solidity
Copy code
function vote() public votingNotEnded {
    require(!hasVoted[msg.sender], "You have already voted");

    hasVoted[msg.sender] = true;
    totalVotes += 1;

    assert(hasVoted[msg.sender] == true); // Ensure the vote was registered
}
endVoting

Allows the owner to end the voting session.

solidity
Copy code
function endVoting() public onlyOwner {
    votingEnded = true;

    // Ensure that voting has ended
    assert(votingEnded == true);
}
resetVoting

Allows the owner to reset the voting session if it has already ended.

solidity
Copy code
function resetVoting() public onlyOwner {
    // Check if the voting is not already in progress
    if (!votingEnded) {
        revert("Voting is still in progress, cannot reset");
    }

    votingEnded = false;
    totalVotes = 0;

    // Reset voting status for all voters (in a real scenario, we would need to reset all mappings)
    // For simplicity, not doing this here as it would require more complex data structures
}
Explanation of require(), assert(), and revert()
require(): Used to validate conditions before executing the function. If the condition is not met, the function execution stops, and any changes are reverted. This is used to check conditions such as whether the voting session has ended or if a user has already voted.

assert(): Used to check for internal errors and validate invariants. If the condition fails, it indicates a bug in the contract, and all changes are reverted. This is used to ensure that the contract's internal state is consistent.

revert(): Used to stop execution and revert all changes if certain conditions are not met. This is useful for complex conditional checks and for providing error messages.

Usage
Deploy the Contract

Deploy the Voting contract to your preferred Ethereum network.
Cast a Vote

Call the vote function to cast a vote. Ensure that you haven't already voted and that the voting session is active.
End Voting

As the owner, call the endVoting function to end the voting session.
Reset Voting

As the owner, call the resetVoting function to reset the voting session if it has ended.
