# Release a new ydbi gem

* bundle exec rake test test_dbd test_dbi
* rake ydbd-pg:clobber_package; rake ydbd-pg:clobber_package; rake ydbi:gem ydbd-pg:gem
* bundle exec gem push pkg/ydbd-*.gem
* bundle exec gem push pkg/ydbi-*.gem

# Description
    The DBI package is a vendor independent interface for accessing databases.
    It is similar, but not identical to, Perl's DBI module.

    Branch by ywesee.com as our patches never got integrated upstream.
    The Gem is named ydbi, but internally we still use dbi as name

# Synopsis

    require 'dbi'
    
    # Connect to a database, old style
    dbh = DBI.connect('DBI:Mysql:test', 'testuser', 'testpwd')
    
    # Insert some rows, use placeholders
    1.upto(13) do |i|
        sql = "insert into simple01 (SongName, SongLength_s) VALUES (?, ?)"
        dbh.do(sql, "Song #{i}", "#{i*10}")
    end 
    
    # Select all rows from simple01
    sth = dbh.prepare('select * from simple01')
    sth.execute
    
    # Print out each row
    while row=sth.fetch do
        p row
    end
    
    # Close the statement handle when done
    sth.finish
    
    # Don't prepare, just do it!
    dbh.do('delete from simple01 where internal_id > 10')
      
    # And finally, disconnect
    dbh.disconnect
        
    # Same example, but a little more Ruby-ish
    DBI.connect('DBI:Mysql:test', 'testuser', 'testpwd') do | dbh |
      
        sql = "insert into simple01 (SongName, SongLength_s) VALUES (?, ?)"
        
        dbh.prepare(sql) do | sth | 
            1.upto(13) { |i| sth.execute("Song #{i}", "#{i*10}") }
        end 
        
        dbh.select_all('select * from simple01') do | row |
            p row
        end
        
        dbh.do('delete from simple01 where internal_id > 10')
      
    end

# Prerequisites
    Ruby 1.8.6 or later is the test target, however you may have success with
    earlier 1.8.x versions of Ruby.

# RubyForge Project
    General information: http://ruby-dbi.rubyforge.org
    Project information: http://rubyforge.org/projects/ruby-dbi/
    Downloads: http://rubyforge.org/frs/?group_id=234

# Installation
    There are many database drivers (DBDs) available.  You only need to install
    the DBDs for the database software that you will be using.

## Gem setup:

    gem install dbi
    # One or more of:
    gem install dbd-mysql
    gem install dbd-pg
    gem install dbd-sqlite3
    gem install dbd-sqlite

If you have a non standard path of postgres use something like

    gem install pg -- --with-pg-config=/usr/local/pgsql-10.1/bin/pg_config

Or if you are using bundler

    bundle config build.pg --with-pg-config=/usr/local/pgsql-10.1/bin/pg_config
    bundle install

## Without rubygems:

    ruby setup.rb config
    ruby setup.rb setup
    ruby setup.rb install

## The bleeding edge:

    git clone git://hollensbe.org/git/ruby-dbi.git
    git checkout -b development origin/development
    
    Also available at
    
    git clone git://github.com/erikh/ruby-dbi.git
      
# Available Database Drivers (DBDs)

## DBD::MySQL
    MySQL
    Depends on the mysql-ruby package from http://www.tmtm.org/mysql or
    available from the RAA.

## DBD::ODBC
    ODBC
    Depends on the ruby-odbc package (0.5 or later, 0.9.3 or later recommended) at
    http://www.ch-werner.de/rubyodbc or available from the RAA.  Works together
    with unix-odbc.

## DBD::OCI8
    OCI8 (Oracle)
    Depends on the the ruby-oci8 package, available on the RAA and RubyForge.

## DBD::Pg
    PostgreSQL
    Depends on the pg package, available on RubyForge.

## DBD::SQLite
    SQLite (versions 2.x and earlier)
    Depends on the sqlite-ruby package, available on rubyforge.

## DBD::SQLite3
    SQLite 3.x
    Depends on the sqlite3-ruby package, available on rubyforge.

# Additional Documentation
    See the directories doc/* for DBI and DBD specific information.
    The DBI specification is at doc/DBI_SPEC.rdoc.
    The DBD specification is at doc/DBD_SPEC.rdoc.

# Articles
## Tutorial: Using the Ruby DBI Module
    http://www.kitebird.com/articles/ruby-dbi.html
   
# Applications
## dbi
    The SQL command line interpreter dbi is available in directory
    bin/. It gets installed by default.

# License

   Copyright (c) 2008 Erik Hollensbe

   Copyright (c) 2005-2006 Kirk Haines, Francis Hwang, Patrick May and Daniel
   Berger.

   Copyright (c) 2001, 2002, 2003, 2004 Michael Neumann <mneumann@ntecs.de>
   and others (see the beginning of each file for copyright holder information).

   All rights reserved.

   Redistribution and use in source and binary forms, with or without
   modification, are permitted provided that the following conditions are met:

   1. Redistributions of source code must retain the above copyright notice,
      this list of conditions and the following disclaimer.
   2. Redistributions in binary form must reproduce the above copyright notice,
      this list of conditions and the following disclaimer in the documentation
      and/or other materials provided with the distribution.
   3. The name of the author may not be used to endorse or promote products
      derived from this software without specific prior written permission.

   THIS SOFTWARE IS PROVIDED 'AS IS' AND ANY EXPRESS OR IMPLIED WARRANTIES,
   INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY
   AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED.  IN NO EVENT SHALL
   THE AUTHOR BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL,
   EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO,
   PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS;
   OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY,
   WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR
   OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF
   ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

   This is the BSD license which is less restrictive than GNU's GPL
   (General Public License).

# Contributors
   
   Pistos
      Too much to specify. Infinite patience and help.

   Christopher Maujean
      Lots of initial help when reviving the project.

   Jun Mukai <mukai@jmuk.org>
      Contributed initial SQLite3 DBD.

   John J. Fox IV
      Lots of help testing on multiple platforms.

   Kirk Haines
      One of the authors of the rewrite effort (January 2006).

   Francis Hwang
      One of the authors of the rewrite effort (January 2006).

   Patrick May
      One of the authors of the rewrite effort (January 2006).

   Daniel Berger
      One of the authors of the rewrite effort (January 2006).

   Michael Neumann
      Original author of Ruby/DBI; wrote the DBI and most of the DBDs.

   Rainer Perl
      Author of Ruby/DBI 0.0.4 from which many good ideas were taken.

   Jim Weirich
      Original author of DBD::Pg.  Wrote additional code (e.g. sql.rb,
      testcases).  Gave many helpful hints and comments. 

   Eli Green
      Implemented DatabaseHandle#columns for Mysql and Pg. 

   Masatoshi SEKI
      For his version of module BasicQuote in sql.rb. 

   John Gorman
      For his case insensitive load_driver patch and parameter parser. 

   David Muse
      For testing the DBD::SQLRelay and for his initial DBD. 

   Jim Menard
      Extended DBD::Oracle for method columns. 

   Joseph McDonald
      Fixed bug in DBD::Pg (default values in method columns). 

   Norbert Gawor
      Fixed bug in DBD::ODBC (method columns) and proxyserver. 

   James F. Hranicky
      Patch for DBD::Pg (cache PGResult#result in Tuples) which increased
      performance by a factor around 100. 

   Stephen Davies
      Added method Statement#fetch_scroll for DBD::Pg. 

   Dave Thomas
      Several enhancements. 

   Brad Hilton
      Column coercing patch for DBD::Mysql. 

   Sean Chittenden
      Originally a co-owner of the project.  Submitted several patches
      and helped with lots of comments.

   MoonWolf
      Provided the quote/escape_byte patch for DBD::Pg, DBD::SQLite patch and
      Database#columns implementation. Further patches. 

   Paul DuBois
      Fixed typos and formatting. Maintains DBD::Mysql. 

   Tim Bates
      Bug fixes for Mysql and DBI.

   Brian Candler
      Zero-padding date/time/timestamps fix. 

   Florian G. Pflug
      Discussion and helpful comments/benchmarks about DBD::Pg async_exec vs.
      exec. 
   
   Oliver M. Bolzer
      Patches to support Postgres arrays for DBD::Pg. 

   Stephen R. Veit
      ruby-db2 and DBD::DB2 enhancements. 

   Dennis Vshivkov
      DBD::Pg patches 

   Cail Borrell from frontbase.com
      For the Frontbase DBD and C interface. 
