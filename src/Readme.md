# Source Directory

Make sure that your source code is in the `src` directory.
import os
import sys
from datetime import datetime

def parse_date(date_str):
  return datetime.strptime(date_str,"%Y-%m-%-d").date()

def read_line_at(file,position):
  file.seek(position)
  if position >0:
    file.readline()
  line_start=file.tell()
  line=file.readline()
  return line_start,line

def find_first_position(file,target_date):
  low=0
  high=os.path.getsize(file.name)
  result=-1
  while low<high:
    mid=(low+high)//2
    line_start,line=read_line_at(file,mid)
    if not line:
      break
    current_date_str=line.decode().split()[0]
    try:
      current_date=parse_date(current_date_str)
    except ValueError:
      continue
    if current_date < target_date:
      low=file.tell()
    else:
      high=mid
      if current_date==target_date:
        result=line_start
  if result ==-1:
    return -1
  file.seek(0)
  while True:
  pos=file.tell()
  line=file.readline()
  if not line:
    break
  current_date_str=line.decode().split()[0]
  try:
    current_date=parse_date(current_date_str)
  except ValueError:
    continue
  if current_date==target_date:
    result=pos
    break

  return result



def main():
  if len(sys.argv)!=2:
    print("usage: pyhton extract_logs.py YYYY-MM-DD")
    sys.exit(1)

  target_date_str=sys.argv[1]
  try:
    target_date=parse_date(target_date_str)

  except ValueError:
    print("Invalid date format . please use YYYY-MM-DD")
    sys.exit(1)

  input_filename="large_log_file.txt"
  output_dir="output"
  output_filename=os.path.join(output_dir, f"output_{target_date_str}.txt")
  os.makedirs(output_dir,exit_ok=True)
  with open(input_filename,'rb')as f:
    first_pos=find_first_position(f,target_date)
    if first_pos==-1:
      with open(output_filename,"w') as out_f:
        pass
      print(f"No logs found for {target_date_str}.")
      return
    f.seek(first_pos)
    with open(output_filename,'wb')as out_f:
      while True:
        line_pos=f.tell()
        line=f.readline()
        if not line:
          break
        current_date_str=line.decode().split()[0]
        try:
          current_date=parse_date(current_date_str)
        except ValueError:
          continue
        if current_date!==target_date:
          break
        out_f.write(line)

  print(f"Logs for {target_date_str} saved to {output_filename}.")

if __name__=="__main__":
  main()
