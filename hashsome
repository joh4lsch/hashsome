#!/bin/bash

function xor()
{
  local res=(`echo "$1" | sed "s/../0x& /g"`)
  shift 1
  while [[ "$1" ]]; do
    local one=(`echo "$1" | sed "s/../0x& /g"`)
    local count1=${#res[@]}
    if [ $count1 -lt ${#one[@]} ]
    then
      count1=${#one[@]}
    fi
    for (( i = 0; i < $count1; i++ ))
    do
     res[$i]=$((${one[$i]:-0} ^ ${res[$i]:-0}))
    done
    shift 1
  done
  printf "%02x" "${res[@]}"
}

function tohex()
{
  HEXVAL=$(hexdump -e '"%X"' <<< "$1")
  echo $HEXVAL
}

R=$(($RANDOM % 10))
# make base64 compatible
TR_SEED=`echo "$2" | tr [A-Z] [a-z] | sed 's\/\.?\g' | sed 's/[{}()]+/b/g' | tr [0-9] [9876543210] | base64`
HEXSEED=`tohex "$TR_SEED"`
HEXPASSWD=`tohex "$1"`
XORED=$(xor "$HEXSEED$R" "$HEXPASSWD")
echo "$XORED" | md5sum | awk '{ print $1 }'
