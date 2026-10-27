#!/usr/bin/env python3
import psutil, os, shutil, time, sys
from rich.console import Console
from rich.panel import Panel
from rich.table import Table
from rich.live import Live

console = Console()

def get_sys_info():
    cpu = psutil.cpu_percent(interval=0.5)
    mem = psutil.virtual_memory()
    disk = shutil.disk_usage("/")
    load = os.getloadavg()
    return cpu, mem.percent, disk.used / (1024**3), disk.total / (1024**3), load

def build_panel():
    cpu, mem, disk_used, disk_total, load = get_sys_info()
    table = Table(show_header=False, box=None, padding=(0, 2))
    table.add_column("Metric", style="cyan", justify="right")
    table.add_column("Value", style="green")
    table.add_row("CPU", f"{cpu}%")
    table.add_row("RAM", f"{mem}%")
    table.add_row("Disk", f"{disk_used:.1f}G / {disk_total:.1f}G")
    table.add_row("Load", f"{load[0]:.2f} {load[1]:.2f} {load[2]:.2f}")
    return Panel(table, title="[bold]pystat[/bold] - Live Monitor", border_style="blue")

def main():
    console.clear()
    with Live(build_panel(), refresh_per_second=2) as live:
        while True:
            try:
                live.update(build_panel())
                time.sleep(0.5)
            except KeyboardInterrupt:
                console.print("\n[yellow]Stopped.[/yellow]")
                break

if __name__ == "__main__":
    main()