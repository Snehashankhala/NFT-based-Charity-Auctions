module MyModule::CharityAuction {

    use aptos_framework::signer;
    use aptos_framework::coin;
    use aptos_framework::aptos_coin::AptosCoin;

    struct Auction has store, key {
        highest_bid: u64,
        highest_bidder: address,
        charity: address,
    }

    public fun create_auction(creator: &signer, charity: address) {
        let auction = Auction {
            highest_bid: 0,
            highest_bidder: signer::address_of(creator),
            charity,
        };
        move_to(creator, auction);
    }

    public fun place_bid(bidder: &signer, auction_owner: address, amount: u64) acquires Auction {
        let auction = borrow_global_mut<Auction>(auction_owner);
        assert!(amount > auction.highest_bid, 1);
        
        let bid_amount = coin::withdraw<AptosCoin>(bidder, amount);
        coin::deposit<AptosCoin>(auction.charity, bid_amount);
        
        auction.highest_bid = amount;
        auction.highest_bidder = signer::address_of(bidder);
    }
}
