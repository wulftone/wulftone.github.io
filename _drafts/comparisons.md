    ƛ irb
    2.1.1 :001 > "woof" == /woof/
    false
    2.1.1 :002 > /woof/ == "woof"
    false
    2.1.1 :003 > "woof" === /woof/
    false
    2.1.1 :004 > /woof/ === "woof"
    true
    2.1.1 :005 > "woof" =~ /woof/
    0
    2.1.1 :006 > /woof/ =~ "woof"
    0
