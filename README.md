# NAME

Finance::Google::Portfolio - Manipulate Google Finance portfolios a little

# VERSION

version 1.04

[![Build Status](https://travis-ci.org/gryphonshafer/Finance-Google-Portfolio.svg)](https://travis-ci.org/gryphonshafer/Finance-Google-Portfolio)
[![Coverage Status](https://coveralls.io/repos/gryphonshafer/Finance-Google-Portfolio/badge.png)](https://coveralls.io/r/gryphonshafer/Finance-Google-Portfolio)

# SYNOPSIS

    use Finance::Google::Portfolio;

    my $fg_portfolio = Finance::Google::Portfolio->new();
    $fg_portfolio->login( user => 'user', passwd => 'passwd' );

    my $fgp = Finance::Google::Portfolio->new(
        user   => 'user',
        passwd => 'passwd',
    );

    for my $holding ( @{ $fgp->portfolio(1) } ) {
        my ( $name, $symbol, $shares ) = @{$holding}{ qw( lname s sh ) };
        printf "%-50s %-7s %4d\n", $name, $symbol, $shares;
    }

    $fgp->add({
        type       => 'buy',
        symbol     => 'AAPL',
        shares     => 42,
        price      => 11.38,
        commission => 5,
    });

    $fgp->watchlist({ list => 'AAPL MSFT EXPE TWTR' });

# DESCRIPTION

This module is an attempt to provide a simple means to manipulate a Google
Finance portfolio, at least a little. Google deprecated its API to Google
Finance, so this module attempts some not-likely-to-be-stable web scraping.
There is very little (read: no) error checking or proper handling of edge-cases.
It's not well tested. I make no warranties or guarantees about this code
what-so-ever. Consequently, this module should probably not be used by anyone,
ever.

# LIBRARY METHODS AND ATTRIBUTES

The following are methods and attributes of the module:

## new

This instantiator is provided by [Moo](https://metacpan.org/pod/Moo). It can optionally accept a username
and password, and if so provided, it will call `login()` automatically.

    my $fgp  = Finance::Google::Portfolio->new;
    my $fgp2 = Finance::Google::Portfolio->new(
        user   => 'user',
        passwd => 'passwd',
    );

## login

This method accepts a username and password for a valid/current Google account,
then attempts to authenticate the user and start up a session.

    $fgp->login( user => 'user', passwd => 'passwd' );

The method returns a reference to the object from which the call was made. And
please note that the authentication takes place via a simple [LWP::UserAgent](https://metacpan.org/pod/LWP::UserAgent)
scrape of a web form. For this to work, [LWP::Protocol::https](https://metacpan.org/pod/LWP::Protocol::https) must be
installed and SSL support must be available.

## portfolio

This method gets a whole lot of data from a given portfolio. With Google Finance,
you can setup multiple portfolios. They're numbered sequentially starting at 1.
The `portfolio()` method accepts an integer representing the portfolio number.
If omitted, it assumes you want portfolio 1.

    my $fgp_portfolio_1_data = $fgp->portfolio;
    my $fgp_portfolio_2_data = $fgp->portfolio(2);

What's returned is a hashref data structure. What you may be most interested
in could be: $data->{portfolio\_view}{portfolio\_table}{cps}, an arrayref of
the current items in the portfolio.

## add

This method allows you to add a transaction to a portfolio. This is buying,
selling, shorting, or covering equities.

    $fgp->add({
        type       => 'buy', # transaction type: buy, sell, etc.
        symbol     => 'AAPL',
        shares     => 42,
        price      => 11.38,
        commission => 5,

        pid   => 1,             # optional; indicates portfolio; defaults to 1
        notes => '',            # optional
        date  => 'May 4, 2015', # optional date; defaults to current day
    });

## watchlist

This method sets your watchlist, which is the items listed in your portfolio.
Note that what's in your watchlist is not the same as what's in your transaction
history. The method requires a "list" parameter which is a space-separated
list of symbols for the watchlist. If you want to remove an item from your
watchlist, you need to provide the whole list of symbols minus the item you
want removed.

    $fgp->watchlist({
        list => 'AAPL MSFT EXPE TWTR',
        pid  => 1, # optional; indicates portfolio; defaults to 1
    });

# SEE ALSO

[Moo](https://metacpan.org/pod/Moo).

You can also look for additional information at:

- [GitHub](https://github.com/gryphonshafer/Finance-Google-Portfolio)
- [CPAN](http://search.cpan.org/dist/Finance-Google-Portfolio)
- [MetaCPAN](https://metacpan.org/pod/Finance::Google::Portfolio)
- [AnnoCPAN](http://annocpan.org/dist/Finance-Google-Portfolio)
- [Travis CI](https://travis-ci.org/gryphonshafer/Finance-Google-Portfolio)
- [Coveralls](https://coveralls.io/r/gryphonshafer/Finance-Google-Portfolio)
- [CPANTS](http://cpants.cpanauthors.org/dist/Finance-Google-Portfolio)
- [CPAN Testers](http://www.cpantesters.org/distro/G/Finance-Google-Portfolio.html)

# AUTHOR

Gryphon Shafer <gryphon@cpan.org>

# COPYRIGHT AND LICENSE

This software is copyright (c) 2018 by Gryphon Shafer.

This is free software; you can redistribute it and/or modify it under
the same terms as the Perl 5 programming language system itself.
